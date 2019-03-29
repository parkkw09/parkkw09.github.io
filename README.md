# 정보

## 윈도우에서 네트워크 드라이브 연결

net drive: \\ip /user:id pw

== 빌드 옵션 ==

- 로그 남기기 : time make 2>&1 tee buildlog.txt
- 라인별 명령어 출력 : make SHELL="sh -x"

== tcpdump 사용법 ==

adb shell su -> root 연결
adb shell tcpdump -i any -p -s 0 -w /sdcard/capture.pcap

== 메모리 덤프 확인하여 함수 이름 확인 ==

arm-none-eabi-addr2line.exe -f -e liblifevibes_quickplayer.so 0x0009f754

== 특정 파일 검색하여 삭제 ==

find . -name ".svn" - type d -exec rm -rf  {} \;
find . -name .git  xargs rm -rf

== 마운트 방법 ==

mount -t cifs -o username=park.kwanwoong,password= //host/host ./local

== 프록시 서버를 따로 만들고 해당 서버로 우회 방법 ==

set  grep proxy
export http_proxy=http//...:8080
unset http_proxy

== 메모리 확인 ==

cat /proc/meminfo

== dash bash change ==

sudo dpkg-reconfigure dash

== 원격 복사 ==

scp -P 2022 parkkw09@172.16.120.150:/home/parkkw09/libs/"$1" .

== MTMO CALL ==

mt 받는거 mo 거는거

== 링크 걸기 ==

ln -s [링크될 경로] [생성될 링크 경로]

== eclipse refresh ==

eclipse -clean -refresh

== 윈도우 압축파일 해제시 한글이 깨질때 ==

unzip -O cp949 name.zip

== ubuntu shortcut desktop icon ==

sudo gnome-desktop-item-edit /usr/share/applications/ --create-new

== a파일 풀기 ==

ar -x libc.a

== dos2unix ==

find . -type f -exec dos2unix {} {} ';'

== open ssl pem view ==

openssl x509 -in keytool_crt.pem -inform pem -noout -text

== mac security ==

맥OS 10.12 시에라부터 사라졌습니다(정상입니다)
아래대로 터미널에서 보안설정을 해제하면 됩니다.
방법은 유틸리티 폴더안의 터미널을 열고
sudo spctl --master-disable 엔터

== java exception site ==

/Users/kwp-mac/Library/Application Support/Oracle/Java/Deployment/security/exception.sites

== ANDROID ==

=== 커맨드로 액티비티 직접 실행 ===

adb shell am start -n com.bns.ptt.daehap.a1/.activities.DemoActivity
adb shell am start -n com.lge.ims/.hidden.IMS_Menu

=== 리눅스 Path 설정(.bashrc 혹은 .profile에 넣는다.) ===

export ANDROID_HOME=${HOME}/tools/android-sdk
export ANDROID_NDK=${HOME}/tools/android-ndk-r13b
export NDK=${ANDROID_NDK}
export ANDROID_NDK_HOME=${ANDROID_NDK}
export PATH=${PATH}:${ANDROID_NDK}/:${ANDROID_HOME}/tools/:${ANDROID_HOME}/platform-tools/

export ANDROID_HOME=/Users/$USER/tools/android-sdk-macosx
export NDK=/Users/$USER/tools/android-ndk-r13b
#export NDK_R14=/Users/$USER/tools/android-ndk-r14b
export NDK_BUNDLE=/Users/$USER/Library/Android/sdk/ndk-bundle
#export ANDROID_NDK=$NDK
export ANDROID_NDK_HOME=$NDK_BUNDLE

export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$NDK:/Users/$USER/tools/etc:/Users/$USER/tools/depot_tools:

export PATH="/usr/local/opt/curl/bin:/usr/local/bin:$PATH"
export PATH="/usr/local/opt/sqlite/bin:$PATH"
export PATH="/usr/local/opt/gettext/bin:$PATH"

export USE_CCACHE=1
ccache -M 50G

=== download ndk ===

wget https://dl.google.com/android/repository/sdk-tools-darwin-3859397.zip
wget https://dl.google.com/android/repository/android-ndk-r13b-darwin-x86_64.zip

=== sdk commandline setting ===

sdkmanager --update

== Git 사용법 ==

=== 신규 생성 ===

mkdir xxx.git
git init --bare --shared

=== 신규 업로드 ===

git init
git add .
git commit -m 'init project'
git remote add origin git://주소
git push -u origin master

=== recursive ===

git clone git://주소 --recursive
git submodule init
git submodule update
git submodule foreach 'git pull' or 'git checkout master'

=== remote branch delete ===

git push origin --delete <branch_name>

=== config ===

git config --list
git config --global user.name
git config --global user.email
git config --global core.autocrlf false
ssh-keygen -t rsa

=== .gitignore ===

a comment - 이 줄은 무시한다.
확장자가 .a인 파일 무시
*.a
윗 줄에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않는다. (느낌표)
!lib.a
루트 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않는다.
/TODO
build/ 디렉토리에 있는 모든 파일은 무시한다.
build/
`doc/notes.txt`같은 파일은 무시하고 doc/server/arch.txt같은 파일은 무시하지 않는다.
doc/*.txt
`doc` 디렉토리 아래의 모든 .txt 파일을 무시한다.
doc/**/*.txt

=== 특정 태그로 코드 점프 ===

git reset --hard TAG
git checkout tag_name
git submodule update --init --recursive

=== TAG ===

git tag -a v1.4 -m "my version 1.4"
git push origin v1.5

=== another remote pull ===

git pull <remote> <branch>

=== If you wish to set tracking information for this branch you can do so with ===

git branch --set-upstream-to=origin/<branch> DEV_KWP

=== merge sample ===

git checkout master
git merge hotfix

=== 원격 저장소로 브랜치를 push ===

git push origin shopping_cart

=== GIT file history check ===

git blame 코드

=== export ===

git archive --format zip --output ~/p929-3rd-party-android_20161111.zip master

== SLICKEDIT ==

=== files ===

*.c;*.cc;*.cpp;*.cp;*.cxx;*.c++;*.h;*.hh;*.hpp;*.hxx;*.h++;*.inl;*.xpm;*.java;*.xml;*.sh;*.cmd;*.txt;*.asm;*.S;*.mk;*.cmake;Makefile;*.properties;*.py

=== exclusive ===

.git/;WORK/;windows_build_fix/;test/;tests/;sample/;patches/;liblinphone*/;libs/;gradle/;.gradle/;gen/;bin/;.externalNativeBuild/;build/;.DS_Store;OUTPUT/;.idea/

== etc1 ==

#!/bin/sh
TODAY="$(date '+%Y%m%d')"
UNDERBAR='_'
DIRNAME="$(basename "$PWD")"
FILENAME=$DIRNAME$UNDERBAR$TODAY
echo "create file in ~/Downloads/$FILENAME"
git archive --format zip --output ~/Downloads/$FILENAME.zip master

== etc2 ==

git add --all
git commit -m 'Modified very simple content.'
git push

== cmake simple cross compile ==

cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../../../../protobuf -DCMAKE_TOOLCHAIN_FILE=${NDK}/build/cmake/android.toolchain.cmake -Dprotobuf_BUILD_SHARED_LIBS=ON -DANDROID_NDK=${NDK} -DANDROID_TOOLCHAIN=clang -DANDROID_ABI=arm64-v8a -DANDROID_STL=c++_shared -DANDROID_LINKER_FLAGS="-landroid -llog" -DANDROID_CPP_FEATURES="rtti exceptions"

== 리눅스에서 특정 라이브러리 파일이 몇비트용인지 확인하기 ==

.a(archive) 파일은 리눅스에서 사용하는 정적 라이브러리로 단순히 .o 파일(object)를 모아놓은 파일이다. 특정 라이브러리가 32/64bit인지 알고 싶어서 스택오버플로우를 찾아봤더니 대부분의 대답이 file 명령어를 쓰라고 나와있다.
file [대상 파일명]
하지만 이 명령어로 .a파일을 조회해보면 “Current ar archive”라는 결과만 출력될 뿐이다.
ar x [.a 파일명] //해당 .a 파일에 들어있는 .o파일을 추출한다.
file [꺼낸 .o파일명] //꺼낸 .o파일의 정보를 확인한다. 아래와 같이 결과가 나온다.
.o : ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
비트수, 컴파일한 CPU, 디버그 정보 제거 유무 등등을 알 수 있다.

== get samples ==

mkdir samples && cd samples
mkdir todo && cd todo
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvvm-databinding todo-mvvm-databinding
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvp-rxjava todo-mvp-rxjava
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvp-clean todo-mvp-clean
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvp-dagger todo-mvp-dagger
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvvm-live todo-mvvm-live
git clone https://github.com/oldergod/android-architecture.git -b todo-mvi-rxjava todo-mvi-rxjava
git clone https://github.com/oldergod/android-architecture.git -b todo-mvi-rxjava-kotlin todo-mvi-rxjava-kotlin
git clone https://github.com/dmytrodanylyk/android-architecture.git -b todo-mvp-kotlin-coroutines todo-mvp-kotlin-coroutines
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvvm-live-kotlin todo-mvvm-live-kotlin
git clone https://github.com/googlesamples/android-architecture.git -b todo-mvp-kotlin todo-mvp-kotlin
cd .. && mkdir tictactoe && cd tictactoe
git clone https://github.com/ericmaxwell2003/ticTacToe.git -b mvc tictactoe-mvc
git clone https://github.com/ericmaxwell2003/ticTacToe.git -b mvp tictactoe-mvp
git clone https://github.com/ericmaxwell2003/ticTacToe.git -b mvvm tictactoe-mvvm
cd .. && mkdir google && cd google
git clone https://github.com/googlesamples/android-architecture-components.git
git clone https://github.com/googlesamples/android-sunflower.git
git clone https://github.com/googlesamples/android-ndk.git
git clone https://github.com/googlesamples/android-viewpager2.git
git clone https://github.com/googlesamples/android-audio-high-performance.git
git clone https://github.com/googlesamples/android-Camera2Basic.git
git clone https://github.com/googlesamples/android-Camera2Video.git
git clone https://github.com/googlesamples/android-Camera2Raw.git
cd ..
git clone https://github.com/kunny/kunny-kotlin-book.git -b dagger-step-2 kunny-kotlin-book-dagger
echo 'clone complete'

https://github.com/AgustaRC/MVVMArchitecture.git
