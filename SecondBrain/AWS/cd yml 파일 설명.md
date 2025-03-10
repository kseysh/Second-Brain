```yaml
name: Deploy to Test Instance

on:
  workflow_dispatch:
    inputs:
      logLevel:
        description: 'Log level'
        required: true
        default: 'warning'
        type: choice
        options:
        - info
        - warning
        - debug
  repository_dispatch:
    types: [deploy-to-test-event]
  push:
    branches: [ develop ]

env:
  SPRING_PROFILES_ACTIVE: dev
  ECR_APP_NAME: ${{ secrets.ECR_APP_NAME }}-dev
  ECR_REPO: ${{ secrets.ECR_HOST }}/${{ secrets.ECR_APP_NAME }}-dev
  ECR_HOST: ${{ secrets.ECR_HOST }}

jobs:
  build:
    name: CD Pipeline 
    runs-on: ubuntu-latest

    steps: # job은 name 별로 끊어서 길행된다.
      - name: Checkout
        uses: actions/checkout@v3 # uses: 이미 만들어져 있는 것을 사용하겠다. 
								  #이건 우분투에 git clone을 받는 것

      - name: Set up JDK 1.17
        uses: actions/setup-java@v3
        with:
          distribution: 'corretto'
          java-version: '17'

      - name: 'Check Java Version'
        run: |
          java --version
          echo 'ecr repo is $ECR_REPO'

      - name: 'Configure AWS credentials'
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID__TEAM_PLAYGROUND }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY__TEAM_PLAYGROUND }}
          aws-region: ap-northeast-2

      - name: 'Get application.yml from AWS S3'
        run: |
          aws s3 cp \
            --region ap-northeast-2 \
            s3://sopt-makers-internal/dev/deploy/application-dev.yml src/main/resources/application.yml

      - name: 'Get key from AWS S3'
        run: |
          aws s3 cp \
            --region ap-northeast-2 \
            s3://sopt-makers-internal/dev/deploy/${{ secrets.APPLE_KEY }} src/main/resources/static/${{ secrets.APPLE_KEY }}

      - name: 'Build with Gradle'
        run: |
          chmod +x ./gradlew
          ./gradlew clean build -x test
        shell: bash

      - name: 'Login to ECR'
        run: |
          aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_HOST

      - name: 'Docker Image Build & Push'
        run: |
          docker build -t $ECR_APP_NAME .
          docker tag $ECR_APP_NAME:latest $ECR_REPO
          docker push $ECR_REPO

      - name: 'Send docker-compose.yml to EC2 Instance'
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.DEV_RELEASE_SERVER_IP }}
          username: ${{ secrets.RELEASE_SERVER_USER }}
          key: ${{ secrets.DEV_RELEASE_SERVER_KEY }}
          source: "./docker-compose.yml"
          target: "/home/ec2-user/playground/"

      - name: 'Send deploy script to EC2 Instance'
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.DEV_RELEASE_SERVER_IP }}
          username: ${{ secrets.RELEASE_SERVER_USER }}
          key: ${{ secrets.DEV_RELEASE_SERVER_KEY }}
          source: "./scripts/"
          target: "/home/ec2-user/playground/"

      - name: 'Docker Container Run'
        uses: appleboy/ssh-action@master

        with:
          host: ${{ secrets.DEV_RELEASE_SERVER_IP }}
          username: ${{ secrets.RELEASE_SERVER_USER }}
          key: ${{ secrets.DEV_RELEASE_SERVER_KEY }}
          script: | 
            cd ~
            sudo docker pull $ECR_REPO
            sudo chmod +x ./playground/scripts/*.sh
            ./playground/scripts/deploy.sh
            docker image prune -f
```