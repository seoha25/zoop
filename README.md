## :pushpin: zoop
> AI 기반 부동산 매물 추천 웹 & 모바일 서비스   
   
</br>   

### 1.제작기간&참여 인원   
* 2025.04.28 ~ 2025.06.27   
* 기업연계 팀 프로젝트(13명)   

</br>

### 2.사용기술   
* JAVA17
* Spring Boot
* PostgreSQL   
* spring Security   
* JWT   
* Spring Data Jpa
* git action
* AWS
       
 </br>     

 ### 3. API 설계 
 ---   
![Image](https://github.com/user-attachments/assets/f0b76edd-9783-4285-80bf-b0949254f41d)   
![Image](https://github.com/user-attachments/assets/13bfb8f8-b9cb-426d-8561-ebd867fc886a)   
![Image](https://github.com/user-attachments/assets/e21c5b9b-659e-448d-aa2a-ba6b92683cc5)   
![Image](https://github.com/user-attachments/assets/fa9a5df3-8ceb-48f8-bc8d-ea20ae57c545)   
![Image](https://github.com/user-attachments/assets/c9d42b74-d017-442a-9d80-e8c4bb3e5e96)   
![Image](https://github.com/user-attachments/assets/028e7b18-8014-4246-84bc-bb7ab92a8087)   
![Image](https://github.com/user-attachments/assets/69a22d91-3a7b-4fdd-8811-ec79ab76aba9)   

</br>   

### 4. 시연영상   
https://github.com/user-attachments/assets/61034b25-8d2b-4627-b9c7-76a1217da6c1

</br>      

### 5.내가 맡은 기능   
  * 필터 기능 : 사용자가 원하는 매물의 거래, 주거, 예산을 선택하면 필터 테이블에 데이터가 저장된다.
  * 채팅 필터 히스토리 기능 : 로그인한 사용자가 채팅방에서 매물조건을 선택시에 필터 저장과 동시에 채팅 필터 히스토리에 데이터가 저장된다.
  * 키워드 필터 히스토리 기능 : 로그인한 사용자가 알림을 통해서 키워드를 등록할 경우 필터 데이터와 동일한 조건인 경우 재사용하고 키워드 필터 히스토리에 저장하고
                              동일한 조건이 없을 경우 필터에 데이터를 저장하면서 키워드 필터 히스토리에도 기록을 한다.     
  * 채팅 목록 조회 및 검색 기능 : 사용자가 로그인한 상태일 때 아무것도 검색하지 않으면 전체 채팅방이 조회되고 검색어를 입력하면 제목이나 채팅내용 가장 최신의 것과 일치하는 키워드로 조회    

<details>
<summary>핵심기능설명펼치기</summary>   

### 6.핵심 트러블 슈팅
#### 6-1. 채팅방 전체조회와 검색을 통한 조회를 하나의 API로 통합
  
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

### 6. 느낀점   
다인원과 함께 프로젝트를 진행하면서 원하는 방향으로 프로젝트를 마무리 하기 위해서는 문서작업이 중요하고 
데이터를 요청하고 응답하는 방식 또한 통일성 있게 미리 규칙을 정하면 프론트엔드 팀과 협업하는 데에 도움이 된다는 것을 배울 수 있었습니다. 

