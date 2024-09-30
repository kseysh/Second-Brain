![](https://blog.kakaocdn.net/dn/cVwo87/btr2xSGmPDY/Uzei7AFFlOVDsMHS0oSaWk/img.png)![](https://blog.kakaocdn.net/dn/cYGEeN/btr2FgTEoYN/M78d4kZlfXHGDgw4XIvK5K/img.png)![](https://blog.kakaocdn.net/dn/qNfCp/btr2FhZhMZb/GrQT1TrV46KL2oxEmywyj0/img.png)
![](https://blog.kakaocdn.net/dn/bUGGZt/btr2CvYoBIR/e4t24LAFJjWcLK93zoe3a0/img.png)

아래 글 추가
```
"cpp": "cd $dir && g++ -std=c++17 $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt"
```


# 디버깅 모드
Terminal > Configure Default Build Task -> **clang++ 활성 파일 빌드**
![](https://blog.kakaocdn.net/dn/tEhlW/btr2Fdo572w/fQQiOPAV5iOhQSeo7gRQ5k/img.png)

1) std=c++17 버전을 쓰겠다. (8번째 줄)
![](https://blog.kakaocdn.net/dn/bRvkW6/btr2xSsQDHE/deFZ5hRz89VrYK8mDMiSt1/img.png)
# task.json
```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: clang++ 활성 파일 빌드",
			"command": "/usr/bin/clang++",
			"args": [
				"-std=c++17",
				"-fcolor-diagnostics",
				"-fansi-escape-codes",
				"-g",
				"${file}",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": "build",
			"detail": "컴파일러: /usr/bin/clang++"
		}
	]
}
```

# launch.json
```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "clang++ - Build and debug active file",
			"type": "cppdbg",
			"request": "launch",
			"program": "${fileDirname}/${fileBasenameNoExtension}",
			"args": [
				"-std=c++17",
				"-fcolor-diagnostics",
				"-fansi-escape-codes",
				"-g",
				"${file}",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}"
			],
			"stopAtEntry": false,
			"cwd": "${workspaceFolder}",
			"environment": [],
			"externalConsole": true,
			"MIMode": "lldb",
			"preLaunchTask": "C/C++: clang++ 활성 파일 빌드"
		}
	]
}
```