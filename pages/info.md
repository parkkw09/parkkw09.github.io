---
layout: page
title: info
permalink: /info
---

[[TIP]]

윈도우에서 네트워크 드라이브 연결
$ net drive: \\ip /user:id pw

tcpdump 사용법
$ adb shell su -> root 연결 $ adb shell tcpdump -i any -p -s 0 -w /sdcard/capture.pcap

메모리 덤프 확인하여 함수 이름 확인
$ arm-none-eabi-addr2line -f -e liblifevibes_quickplayer.so 0x0009f754

특정 파일 검색하여 삭제
$ find . -name ".svn" - type d -exec rm -rf {} \; $ find . -name .git xargs rm -rf

마운트 방법
$ mount -t cifs -o username=park.kwanwoong,password= //host/host ./local

프록시 서버를 따로 만들고 해당 서버로 우회 방법
$ set grep proxy $ export http_proxy=http//...:8080 $ unset http_proxy

메모리 확인
$ cat /proc/meminfo

MTMO CALL
mt 받는거 mo 거는거

링크 걸기
$ ln -s [링크될 경로] [생성될 링크 경로]

윈도우 압축파일 해제시 한글이 깨질때
$ unzip -O cp949 name.zip

ubuntu shortcut desktop icon
$ sudo gnome-desktop-item-edit /usr/share/applications/ --create-new

a파일 풀기
$ ar -x libc.a

dos2unix
$ find . -type f -exec dos2unix {} {} ';'

open ssl pem view
$ openssl x509 -in keytool_crt.pem -inform pem -noout -text

java exception site
$ /Users/kwp-mac/Library/Application Support/Oracle/Java/Deployment/security/exception.sites

리눅스에서 특정 라이브러리 파일이 몇비트용인지 확인하기
.a(archive) 파일은 리눅스에서 사용하는 정적 라이브러리로 단순히 .o 파일(object)를 모아놓은 파일이다. 특정 라이브러리가 32/64bit인지 알고 싶어서 스택오버플로우를 찾아봤더니 대부분의 대답이 file 명령어를 쓰라고 나와있다.

file [대상 파일명]
하지만 이 명령어로 .a파일을 조회해보면 “Current ar archive”라는 결과만 출력될 뿐이다.

ar x [.a 파일명]
해당 .a 파일에 들어있는 .o파일을 추출한다. file [꺼낸 .o파일명] //꺼낸 .o파일의 정보를 확인한다. 아래와 같이 결과가 나온다. 비트수, 컴파일한 CPU, 디버그 정보 제거 유무 등등을 알 수 있다.
.o : ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped

Homebrew 사용시 이전버전 가져오기
brew edit xxx 로 저장소 기록을 변경한다.
homebrew github에서 히스토리를 통해 정보를 찾은 다음 교체해주면 된다.
brew tap-new company/team; brew extract --version 0.68.3 hugo company/team; brew install company/team/hugo@0.68.3

FFMPEG
$ ffmpeg -i inputfile.mkv -c:v copy -c:a ac3 outputfile.mkv
$ ffmpeg -i 00008.m2ts -map 0:8 -c:s copy "08.sup"
$ ffmpeg -i src -c: copy dst

[[빌드 옵션]]

로그 남기기
$ time make 2>&1 | tee buildlog.txt

라인별 명령어 출력
$ make SHELL="sh -x"

[[WebRTC 빌드]]

https://webrtc.googlesource.com/src/+/refs/heads/master/docs/native-code/index.md
여기 나온거 그대로 따라하자.

tools_webrtc/android/build_aar.py --build-dir out
다 받은 소스에 위 명령을 실행하면 라이브러리만 빌드 가능.

m88 브랜치를 받아서 빌드하는 방법

$ fetch --nohooks webrtc_android
$ cd src
$ git checkout -b m88 refs/remotes/branch-heads/4324
$ gclient sync -D
$ gn gen out/Debug --args='target_os="android" target_cpu="arm"'
$ autoninja -C out/Debug

[[ANDROID]]

커맨드로 액티비티 직접 실행
$ adb shell am start -n com.bns.ptt.daehap.a1/.activities.DemoActivity
$ adb shell am start -n com.lge.ims/.hidden.IMS_Menu

download ndk
$ wget https://dl.google.com/android/repository/sdk-tools-darwin-3859397.zip
$ wget https://dl.google.com/android/repository/android-ndk-r13b-darwin-x86_64.zip

sdk commandline setting
$ sdkmanager --update
$ sdkmanager --sdk_root=${ANDROID_HOME} "platform-tools" "platforms;android-29"

사인키 생성 수동
$ keytool -genkey -v -keystore my-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-alias

사인키 확인
$ jarsigner -verify -verbose -certs name.apk
$ keytool -list -printcert - jarfile name.apk

앱 서명 수동
$ zipalign -v -p 4 unsigned.apk unsigned-align.apk
$ apksigner sign --ks keystore.jks --out signed.apk unsigned-align.apk

앱 서명 확인
$ apksigner verify name.apk

[[Git 사용법]]

신규 생성
$ mkdir xxx.git $ git init --bare --shared

신규 업로드
$ git init $ git add .
$ git commit -m 'init project'
$ git remote add origin git://주소
$ git push -u origin master

recursive
$ git clone git://주소 --recursive
$ git submodule init
$ git submodule update
$ git submodule foreach 'git pull' or 'git checkout master'

remote branch delete
$ git push origin --delete

config
$ git config --list
$ git config --global user.name
$ git config --global user.email
$ git config --global core.autocrlf false
$ ssh-keygen -t rsa

.gitignore
a comment - 이 줄은 무시한다.
*.a - 확장자가 .a인 파일 무시
!lib.a - 윗 줄에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않는다. (느낌표)
/TODO - 루트 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않는다.
build/ - build/ 디렉토리에 있는 모든 파일은 무시한다.
doc/*.txt - doc/notes.txt같은 파일은 무시하고 doc/server/arch.txt같은 파일은 무시하지 않는다.
doc/**/*.txt - doc 디렉토리 아래의 모든 .txt 파일을 무시한다.

.idea와 같이 설정 파일이 ignore에 등록을 해도 추적이 될 경우
깃의 경우 ignore에 등록을 하더라도 이미 저장이 된 상태라면 무시가 안될수 있다.
git clean -f -d .idea

특정 태그로 코드 점프
$ git reset --hard TAG
$ git checkout tag_name
$ git submodule update --init --recursive

TAG - add
$ git tag -a v1.4 -m "my version 1.4"
$ git push origin v1.5

TAG - del
$ git tag -d v1.4
$ git push origin :v1.4

TAG - checkout
$ git checkout tags/v1.4 -b v1.4

another remote pull
$ git pull origin xxx

If you wish to set tracking information for this branch you can do so with
$ git branch --set-upstream-to=origin/ DEV_KWP

merge sample
$ git checkout master $ git merge hotfix

원격 저장소로 브랜치를 push
$ git push origin shopping_cart

GIT file history check
$ git blame 코드

export
$ git archive --format zip --output ~/p929-3rd-party-android_20161111.zip master

[[Make shell]]

get archive

#!/bin/sh
TODAY="$(date '+%Y%m%d')"
UNDERBAR='_'
DIRNAME="$(basename "$PWD")"
FILENAME=$DIRNAME$UNDERBAR$TODAY
echo "create file in ~/Downloads/$FILENAME"
git archive --format zip --output ~/Downloads/$FILENAME.zip master

simplepush

git add --all git commit -m 'Modified very simple content.' git push

cmake simple cross compile

cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../../../../protobuf -DCMAKE_TOOLCHAIN_FILE=${NDK}/build/cmake/android.toolchain.cmake -Dprotobuf_BUILD_SHARED_LIBS=ON -DANDROID_NDK=${NDK} -DANDROID_TOOLCHAIN=clang -DANDROID_ABI=arm64-v8a -DANDROID_STL=c++_shared -DANDROID_LINKER_FLAGS="-landroid -llog" -DANDROID_CPP_FEATURES="rtti exceptions"

리눅스 Path 설정
(.bashrc 혹은 .profile에 넣는다.)
export ANDROID_HOME=${HOME}/tools/android-sdk-linux
export NDK_R13=${HOME}/tools/android-ndk-r13b
export NDK_R16=${HOME}/tools/android-ndk-r16b
export NDK_R19=${HOME}/tools/android-ndk-r19c
export NDK_R21=${HOME}/tools/android-ndk-r21e
export NDK=$NDK_R21
export PATH=${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin
export PATH=${PATH}:${ANDROID_HOME}/platform-tools:${NDK}
export PATH=${PATH}:${HOME}/tools/LogFilter:${HOME}/tools/jdk1.8.0_261/bin
export PATH=${PATH}:${HOME}/tools/etc:${HOME}/tools/depot_tools

.zshrc
export ANDROID_HOME=$HOME/tools/android-sdk-macosx
export NDK_R13=$HOME/tools/android-ndk-r13b
export NDK_R16=$HOME/tools/android-ndk-r16b
export NDK_R19=$HOME/tools/android-ndk-r19c
export NDK_R21=$HOME/tools/android-ndk-r21
export NDK=$NDK_R21
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:
export PATH=$PATH:$ANDROID_HOME/platform-tools:$NDK:
export PATH=$PATH:$HOME/tools/etc:$HOME/tools/depot_tools:

def homePath = System.properties['user.home']
def localProps = new Properties()
localProps.load(new File(homePath + '/Documents/private/key/app_props.properties').newDataInputStream())

This theme is completely free and open source software. You may use it however you want, as it is distributed under the [MIT License](http://choosealicense.com/licenses/mit/). If you are having any problems, any questions or suggestions, feel free to [tweet at me](https://twitter.com/intent/tweet?text=My%question%about%Millennial%is:%&amp;via=paululele), or [file a GitHub issue](https://github.com/lenpaul/Millennial/issues/new).
