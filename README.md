<h1 align="center">🏠 zoop — 공인중개사를 위한 AI 기반 부동산 플랫폼</h1>

<p align="center">
  <a href="#-프로젝트-개요">프로젝트 개요</a> •
  <a href="#-기간--팀-구성">기간 & 팀 구성</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-api--아키텍처">API / 아키텍처</a> •
  <a href="#-시연-영상">시연 영상</a> •
  <a href="#-내-담당-기능">내 담당 기능</a> •
  <a href="#-핵심-트러블슈팅">핵심 트러블슈팅</a> •
  <a href="#-느낀점--회고">느낀점 / 회고</a>
</p>

<p align="center">
  <img alt="Java" src="https://img.shields.io/badge/Java-17+-red?logo=openjdk&logoColor=white">
  <img alt="Spring Boot" src="https://img.shields.io/badge/Spring%20Boot-3.x-6DB33F?logo=springboot&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white">
  <img alt="Security" src="https://img.shields.io/badge/Spring%20Security-JWT-2c3e50">
  <img alt="AWS" src="https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws&logoColor=white">
</p>

---

## 📌 프로젝트 개요
공인중개사 업무에 특화된 **AI 기반 부동산 플랫폼**으로,  
사용자 맞춤 **필터/채팅 히스토리**를 통해 매물 탐색 효율을 높였습니다.

---

## ⏱ 기간 & 팀 구성
- **기간:** 2025.04.28 ~ 2025.06.27 (기획 포함)
- **형태:** 기업 연계 **팀 프로젝트 (총 13명)**

---

## 🧰 Tech Stack
| 분류 | 기술 |
|---|---|
| Language | **Java**, **Python** |
| Backend | **Spring Boot**, **Spring Security**, **JPA**|
| Auth | **JWT** |
| DB | **PostgreSQL** |
| Infra | **AWS (EC2)** |
| Collaboration | **GitHub**, **Notion**, **Figma** |

---

## 🧩 API / 아키텍처
아래 문서/다이어그램은 주요 API 설계 및 서버-클라이언트 상호작용을 보여줍니다.

<img width="962" height="377" alt="Image" src="https://github.com/user-attachments/assets/03325bc4-254b-431b-8f9b-8604d2d9790c" />
<img width="961" height="318" alt="Image" src="https://github.com/user-attachments/assets/a45444f7-f35f-43e6-945b-733fe62704aa" />
<img width="935" height="732" alt="Image" src="https://github.com/user-attachments/assets/3fa724f3-d44a-4612-b6aa-84ac7b65c704" />
<img width="942" height="852" alt="Image" src="https://github.com/user-attachments/assets/35f033c5-c1d7-4998-82d3-6080a2839856" />
<img width="935" height="904" alt="Image" src="https://github.com/user-attachments/assets/867a3ed0-ce20-4699-ba23-6256e906d25a" />
<img width="912" height="374" alt="Image" src="https://github.com/user-attachments/assets/9cc4511e-44d7-4185-b845-cdf3bc6d3942" />


---

## 🎬 시연 영상
- 링크: https://github.com/user-attachments/assets/61034b25-8d2b-4627-b9c7-76a1217da6c1

---

## 👤 내 담당 기능
- **필터 기능**: 거래/주거/예산 선택 시 **필터 테이블 저장**(재사용 가능 구조).
- **채팅 필터 히스토리**: 로그인 사용자가 채팅방에서 조건 선택 시 **동시 기록**.
- **채팅 목록 조회/검색**: 로그인 사용자의 전체 채팅방 조회 + **제목/내용 키워드 최신 매칭** 검색.


---

## 🧪 핵심 트러블슈팅
### 1) “전체조회”와 “검색조회”를 **단일 API**로 통합
> UX 단순화 & API 표면적 축소. 빈 검색어는 “전체조회”, 값이 있으면 “검색조회”로 처리.
  
<details>      
<summary>기존코드</summary>      
<pre>
    // 로그인한 사용자의 채팅방 중에서 제목이나 내용으로 조회
    @GetMapping("/search")
    public ResponseEntity<?> searchChatRoom(@AuthenticationPrincipal LoginUser loginUser,
                                            @RequestParam String searchText) {
        Long userId = Long.valueOf(loginUser.getUsername());
        List<ChatRoomSearchDto> chatRooms = chatService.searchChatRooms(userId, searchText);

        return ResponseEntity.ok(
                ResponseResult.success(
                        HttpStatus.OK,
                        GET_SUCCESS.getMessage(),
                        chatRooms
                )
        );
    }
</pre>
   
</details>   

<details>
<summary>개선된 코드</summary>
<pre>
 
        // 로그인한 사용자의 채팅방 중에서 제목이나 내용으로 조회
    @GetMapping
    public ResponseEntity<?> searchChatRoom(@AuthenticationPrincipal LoginUser loginUser,
                                            @RequestParam(required = false) String searchText) {
        Long userId = Long.valueOf(loginUser.getUsername());
        List<ChatRoomRequestDto> chatRooms;

        if (searchText == null || searchText.trim().isEmpty()) {
            chatRooms = chatService.getAllChatRooms(userId); // 전체 채팅방 조회

        } else {
            chatRooms = chatService.searchChatRooms(userId, searchText); // 검색한 결과 조회
        }
        return ResponseEntity.ok(
                ResponseResult.success(
                        HttpStatus.OK,
                        GET_SUCCESS.getMessage(),
                        chatRooms
                )
        );
    }
 
</pre>   
</details>   
</br>

---

## 💡 느낀점 / 회고
- 문서작업의 중요성: 다인원 협업에서 일관된 API 스펙/에러 규칙/응답 포맷을 선제 정의하면 프론트 협업 효율 높임.

