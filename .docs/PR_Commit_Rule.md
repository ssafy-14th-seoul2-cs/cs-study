# Pull Request & Commit 규칙

## Pull Request
- PR 제목은 '(이름) / 1주차 학습 내용 정리'와 같이 작성합니다.
- PR 내용에는 자유롭게 학습한 주제별로 요약하여 작성합니다.
- 해당하는 주제 라벨을 선택합니다.
- 예시
<img width="1137" height="712" alt="Image" src="https://github.com/user-attachments/assets/e8479e45-709c-4e31-a300-8818670d6f27" />

## Commit 규칙
- type
    - `docs` : 새로운 정리 자료 추가 / 기존 문서에 내용 추가
    - `fix` : 오타, 그림/링크/표 경로 수정
    - `chore` : 폴더 생성, 파일 이름 변경, 레포 구조 관리
- scope
    - 주차 + 주제 형태로 작성 : `week1-CompArch`, `week3-OS`, `week6-network` 등
        |주제|scope|
        |---|---|
        |컴퓨터 구조|`CompArch`|
        |운영체제|`OS`|
        |네트워크|`Network`|
        |데이터베이스|`DB`|
        |디자인 패턴|`DP`|
- subject
    - 구체적인 작업 내용을 작성합니다. (50자 내외)
    - 어떤 작업인지 알아볼 수 있을 정도로만 자유롭게 작성합니다.
- 예시
    - `docs(week1-CompArch) : 폰 노이만 구조 정리`
    - `fix(week3-OS) : 프로세스 상태 표 오타 수정`
    - `chore(week4-CompArch) : 'Cash_Memory.md' -> 'Cache_Memory.md' 파일명 수정`