```shell
<configuration>
    <!-- 콘솔에 로그 출력 (nohup.out에서 확인 가능) -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 로그 파일 저장 (~/log/app.log) -->
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>${user.home}/log/app.log</file>  <!-- ✅ 홈 디렉토리에 저장 -->
        <append>true</append>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- Hibernate SQL 로그 설정 -->
    <logger name="org.hibernate.SQL" level="DEBUG"/>
    <logger name="org.hibernate.type.descriptor.sql" level="TRACE"/>

    <!-- 전체 로그 설정 (콘솔 + 파일에 출력) -->
    <root level="INFO">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```

## file-error-appender.xml


## 