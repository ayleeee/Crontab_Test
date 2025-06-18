# Crontab_Test 💪
Crontab을 활용한 TIL(Today I Learned) 정리

# Crontab 이란?
유닉스 계열 컴퓨터 운영 체제의 시간 기반 잡 스케줄러. 특정 시간에 특정 프로그램을 실행시키기 위해 사용.

# Crontab 명령어
```
# 크론탭 설정을 할 수 있는 에디터 화면 출력
crontab -e

# 현재 크론탭 작업 목록 확인
crontab -l

# 설정되어 있는 크론탭 작업 모두 삭제
crontab -r

# 크론탭 작업 설정하는 법
* * * * * /workdir/crontab.sh
분(0-59) 시간(0-23) 일(1-31) 월(1-12) 요일(0-7)

예시) 매주 월요일 오전 1시, 오후 1시에 crontab.sh 실행
0 1,13 * * 1 /workdir/crontab.sh

# 크론탭 작업의 실행 결과를 로그 파일로 저장하는 법
0 1,13 * * 1 /workdir/crontab.sh >> /workdir/cron.log 2>&1
cf) 2>&1 : 표준 에러를 표준 출력으로 합치는 것
(정상 출력과 에러 메세지 모두 cron.log에 기록)
```


# 개요📢
하루동안의 Woori Fisa 수업과정을 통해 나온 결과물들을 하나의 폴더에 하루마다 저장하여 정리해주는 자동화 시스템을 만들어보고자 하여 기획하게 되었습니다.

# 폴더 생성 sh 생성 🔨
```bash
#!/bin/bash

# 폴더가 생성될 경로
DIR="/home/username"

# 오늘 날짜를 'YYYY-MM-DD' 형식으로 가져옴
TODAY=$(date +"%Y-%m-%d")

# 폴더가 이미 존재하는지 확인하고, 없으면 생성
if [ ! -d "$DIR/$TODAY" ]; then
    mkdir "$DIR/$TODAY"
fi
```
# 파일 이동 sh 생성 🔨
```bash
# move_file.sh 생성
#!/bin/bash

# 파일들이 위치한 디렉토리 (파일을 찾을 경로)
SOURCE_DIR="/home/username"

# 폴더가 생성될 경로 (오늘 날짜 폴더가 위치한 경로)
DEST_DIR="/home/username"

# 오늘 날짜를 'YYYY-MM-DD' 형식으로 가져옴
TODAY=$(date +"%Y-%m-%d")

# 오늘 날짜 폴더가 없으면 생성
if [ ! -d "$DEST_DIR/$TODAY" ]; then
    mkdir "$DEST_DIR/$TODAY"
fi

# 오늘 작성된 파일들을 찾고 폴더로 이동
find "$SOURCE_DIR" -type f -newermt $(date +%Y-%m-%d) ! -newermt $(date +%Y-%m-%d -d tomorrow) -exec mv {} "$DEST_DIR/$TODAY" \;
```

# Cron tab 설정 💻
```bash
// 오전 7시에 create_folder.sh 실행
0 7 * * *  /home/username/create_folder.sh >/dev/null 2>&1
// 오후 11시 59분에 move_files.sh 실행
59 23 * * * /home/username/move_files.sh >/dev/null 2>&1 
```

# 결과 🐱‍💻
### Crontab 적용 전
![크론탭 설정 이미지 1](crontab_image/image.png)
</br>
### Crontab 적용 후
![크론탭 설정 이미지 2](crontab_image/image2.png)
</br>
