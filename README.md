# Cron을 이용한 백업 자동화 📂

이 프로젝트는 Unix 기반 시스템에서 쉘 스크립트와 cron 작업을 사용하여 자동화된 백업 시스템을 설정하는 방법을 보여줍니다. 디렉토리 구조 생성, 백업 스크립트, 그리고 일일 백업 스케줄링을 포함합니다.

## 목차  📑

- [프로젝트 구조](#프로젝트-구조)
- [설정](#설정)
  - [디렉토리 구조 생성](#디렉토리-구조-생성)
  - [백업 스크립트](#백업-스크립트)
  - [Cron 작업 구성](#cron-작업-구성)
- [사용법](#사용법)
- [파일 설명](#파일-설명)

## 프로젝트 구조🗂️
![](https://velog.velcdn.com/images/yuwankang/post/f51e8d19-8b29-499f-80c0-29844eac5ce6/image.png)

```
project/
│
├── infra/
│   ├── fisa1/
│   ├── fisa2/
│   ├── fisa3/
│   ├── fisa4/
│   └── fisa5/
│
├── infra-back-up/
│
├── backup_infra.sh
└── infra.sh
```

## 설정⚙️

### 디렉토리 구조 생성 🏗️

1. `infra.sh`라는 이름의 스크립트 생성:

```bash
#!/bin/bash
# infra 디렉토리가 없으면 생성
if [ ! -d "./infra" ]; then
  mkdir ./infra
fi
# fisa1부터 fisa5까지 하위 디렉토리 생성
for i in {1..5}; do
  dir_name="./infra/fisa$i"
  if [ ! -d "$dir_name" ]; then
    mkdir "$dir_name"
    echo "$dir_name 디렉토리가 생성되었습니다."
  else
    echo "$dir_name 디렉토리가 이미 존재합니다."
  fi
done
```
![](https://velog.velcdn.com/images/yuwankang/post/293fbf87-f05e-4aca-b65b-e5a9efa971ff/image.png)

2. 스크립트 실행:

```bash
bash infra.sh
```
![](https://velog.velcdn.com/images/yuwankang/post/b72c1a26-a684-44bb-a27c-e7e895de25ca/image.png)

### 백업 스크립트🛠️

`backup_infra.sh`라는 이름의 스크립트 생성:
![](https://velog.velcdn.com/images/yuwankang/post/30f18210-627a-4788-902d-74fc4ada4aba/image.png)
```bash
#!/bin/bash

# 백업 경로 설정
backup_dir="/home/username/step02/infra-back-up"
source_dir="/home/username/step02/infra"

# 현재 날짜로 백업 파일 이름 생성
date=$(date +'%Y%m%d')
backup_file="$backup_dir/backup_infra_$date.tar.gz"

# 백업 수행
tar -czf "$backup_file" -C "$source_dir" .

# 결과 출력
if [ $? -eq 0 ]; then
  echo "백업 성공: $backup_file"
else
  echo "백업 실패"
fi
```
![](https://velog.velcdn.com/images/yuwankang/post/d4b18068-4288-40c0-bbc3-c98a0d7e7efa/image.png)
![](https://velog.velcdn.com/images/yuwankang/post/4251c9eb-dd72-47ec-82d3-a7427771604b/image.png)
### Cron 작업 구성⏰

1. crontab 편집:

```bash
crontab -e
```

2. 매일 오전 9시에 백업을 실행하기 위해 다음 줄 추가:

![](https://velog.velcdn.com/images/yuwankang/post/4efc7590-20ab-4218-9638-35504aa21ab8/image.png)

```
0 9 * * * /home/username/step02/backup_infra.sh
```

3. 실행 권한 부여:

![](https://velog.velcdn.com/images/yuwankang/post/90d33a84-e114-4eaa-bf59-736374868dbe/image.png)
```bash
chmod +x /home/username/step02/backup_infra.sh
```

![](https://velog.velcdn.com/images/yuwankang/post/b8ebe85f-3b7c-42cf-9c6b-223a07b6dc94/image.png)
4. 백업 디렉토리 생성:

```bash
mkdir -p /home/username/step02/infra-back-up
```

![](https://velog.velcdn.com/images/yuwankang/post/6f423c10-ec49-47db-91ce-cd2d37df4802/image.png)
## 사용법 🖥️

이 시스템은 매일 오전 9시에 `infra` 디렉토리의 백업을 자동으로 생성합니다. 백업은 `infra-back-up` 디렉토리에 저장되며, 파일 이름에는 백업 날짜가 포함됩니다.

수동으로 백업을 실행하려면 다음 명령어를 실행하세요:

```bash
bash backup_infra.sh
```

## 파일 설명 📋

- `infra.sh`: 초기 디렉토리 구조를 생성하는 스크립트.
- `backup_infra.sh`: 백업 작업을 수행하는 스크립트.
- `infra/`: 백업할 하위 디렉토리들을 포함하는 디렉토리.
- `infra-back-up/`: 백업 파일들이 저장되는 디렉토리.

---

## 백업 완료👍
![image](https://github.com/user-attachments/assets/2322f412-accd-4531-9a4b-9a28654488b2)

