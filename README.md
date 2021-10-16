<img width="957" alt="스크린샷 2021-10-13 오전 11 57 21" src="https://user-images.githubusercontent.com/86150470/137059368-5bc672fa-003c-4210-a8be-269744e7cc6e.png">
<img width="954" alt="스크린샷 2021-10-13 오전 11 58 09" src="https://user-images.githubusercontent.com/86150470/137059441-4e84d5a0-5fec-4ba2-ab9d-27bff7d9c3e1.png">

<img width="952" alt="스크린샷 2021-10-13 오전 11 58 16" src="https://user-images.githubusercontent.com/86150470/137059447-890bd2bd-86d4-41f7-aefd-1ab8a6c61e1f.png">

테이블 설명
1. member - 멤버테이블

2. hotel -  호텔고유번호pk ,주소, 가격(객실 최저가를 가져옴) , 썸네일 filename ....

3. room - room 고유번호 pk, 호텔 고유번호 fk , 룸 가격, 룸 썸네일 ...

4. image - 룸idx fk , file name     룸의 이미지들 

5. total_point - 호텔의 평점, 리뷰 개수    리뷰를 작성할 때  사용자가입력한 값과 기존 호텔평점 기존 리뷰 카운트를 이용해 연산 

6. service_count -  객실 예약시 트리거로 카운트 올라감  호텔 검색시 우선순위를 위해 사용 

7. hashtag - 해쉬태그를 이용한 검색 기능 

8. search - 검색 키워드를 저장해서 연관검색어,추천검색어 기능에 활용 

9. service - 예약 테이블.   리뷰 작성상태가 포함되어있음.  예약한 사람이 예약1건당 1번씩 리뷰를 작성 할 수 있음

10.  coupon , member_coupon - 쿠폰테이블 회원가입시 트리거로 10% 할인쿠폰 지급 

11.  review - 리뷰 테이블 

외  join 한 view 테이블 4개 

<img width="939" alt="스크린샷 2021-10-13 오전 11 50 51" src="https://user-images.githubusercontent.com/86150470/137068856-2392f816-053e-4a80-adf9-128b6812b01d.png">

예약시 service_count 테이블에 count 증가  트리거 

<img width="1013" alt="스크린샷 2021-10-13 오전 11 50 57" src="https://user-images.githubusercontent.com/86150470/137068935-6afa3035-0599-48c2-95ba-84c017879e13.png">

회원가입시 10% 쿠폰 지급 트리거 

<img width="1063" alt="스크린샷 2021-10-13 오전 11 50 27" src="https://user-images.githubusercontent.com/86150470/137069026-70a5d517-3a72-4ad8-b4c2-ec149f6afe9a.png">

호텔 등록시 total_point 리뷰갯수0,평점0 으로 insert , service_count 테이블 insert 




기능설명 - 


메인페이지 - 회원가입시 설정한 선호지역을 가져와 그 지역의 평점 높은 순 으로 인기호텔 top 5 를 보여줌
                    비회원 시에  회원가입, 로그인 유도 컨텐츠 

                     인기 검색어 컨텐츠 ,  date객채를 이용해 현재 계절을 고려하여 계절별 영상 반영 

검색 페이지 - 검색 컴퍼넌트를 재활용하기위해 따로 분리함.  keyup 이벤트 ajax 로 추천,연관 검색어 
                     검색 많이 된 순으로 노출 시킴.  검색 호텔 정렬기준 평점과 예약많이된 순서

호텔 상세페이지 -  야놀자 url 을 본따서  path 방식으로 url 을 만듬   check_in check_out 은 디폴트 오늘과 내일 
                          이고 세션에 저장해둠. 캘린더로 수정 시 세션에 있는 check_in, check_out 을 수정 
                           check_in,check_out 을 기준으로 객실 예약 상태 반환 .  호텔의 리뷰와  같은 지역  호텔 탑 5 를 
                         불름 . 카카오맵에 마커를 등록하여  마커 클릭 이벤트로 근처 호텔 상세페이지 이동 가능 

룸 상세페이지 - 룸 상세이미지들 과 예약 처리, 예약이 마감된 상태이면 날짜변경, 다른 룸 보기로 이동 

예약 페이지 - 룸 가격과 숙박 일수로 총 가격과  쿠폰 사용시 실시간 가격 렌더링 

마이페이지 - 나의 예약내역에서 check_out 이 오늘 기준으로 지나야 리뷰 작성가능 
                    나의 리뷰에서 수정삭제 가능 , 호텔 등록 , 어드민 페이지, qna페이지 



리뷰 페이지 - 날짜 정렬로 10개씩 불러옴 스크롤 ajax 이벤트  (나의 리뷰, 호텔리뷰 ) 쿼리문 if 을 사용해 재활용함
 
<img width="567" alt="스크린샷 2021-10-13 오전 12 53 02" src="https://user-images.githubusercontent.com/86150470/137070433-7cb9e4aa-ea95-428f-a726-5c0928ef35ac.png">

<img width="670" alt="스크린샷 2021-10-13 오전 12 52 58" src="https://user-images.githubusercontent.com/86150470/137070421-8635c3f6-2054-4031-a29b-51789566ac0a.png">






룸 예약 상태 반환 쿼리문 
<img width="885" alt="스크린샷 2021-10-13 오전 12 52 44" src="https://user-images.githubusercontent.com/86150470/137070278-0dcd3913-a2e6-41a4-a774-0f82dc9b60bd.png">






캘린더 생성 스크립트 

<img width="1001" alt="스크린샷 2021-10-13 오전 12 55 49" src="https://user-images.githubusercontent.com/86150470/137070484-d5411517-eb99-44e2-aed0-4acecebc825e.png">
                           


호텔 등록시 해쉬태그 추가기능 스윗얼럿과 에니메이션 사용  최대 5개  해쉬태그 벳지 클릭시 삭제됨 
<img width="946" alt="스크린샷 2021-10-13 오전 12 53 59" src="https://user-images.githubusercontent.com/86150470/137070581-90053428-5f0e-4306-a774-03caf705b8e4.png">





프로젝트중 어려웠던 부분 

호텔 등록과  룸 상태 반환 
호텔 등록시  호텔의 룸들, 각 룸 마다 이미지들  인 2차원 배열형식으로  form 전송 
뷰 딴에 방 갯수와 방마다 사진들, 사진들 갯수 전송
서버딴에  이차원 배열로 각 방에 맞게 이미지 받아옴 

룸상태반환 check_in , check_out 으로 이미 예약된 컬럼과 비교할때 어려웠음 




구현 페이지 이미지 



<img width="481" alt="스크린샷 2021-10-13 오후 2 08 46" src="https://user-images.githubusercontent.com/86150470/137071173-89e883a7-ea44-4cf8-bfcf-427424b3ccef.png">
<img width="701" alt="스크린샷 2021-10-13 오후 2 08 59" src="https://user-images.githubusercontent.com/86150470/137071178-d029b04c-3507-425f-9c3a-2a2dafa29e6c.png">
<img width="720" alt="스크린샷 2021-10-13 오후 2 09 06" src="https://user-images.githubusercontent.com/86150470/137071179-86834b0e-82df-4393-b8a9-be7ff43b97f1.png">
<img width="461" alt="스크린샷 2021-10-13 오후 2 09 10" src="https://user-images.githubusercontent.com/86150470/137071190-e2facd8b-d491-4e5a-adde-8546eb3b643b.png">
<img width="442" alt="스크린샷 2021-10-13 오후 2 09 15" src="https://user-images.githubusercontent.com/86150470/137071205-e0b3f69d-e951-42a4-899c-c63a9e30e2a6.png">
<img width="464" alt="스크린샷 2021-10-13 오후 2 09 21" src="https://user-images.githubusercontent.com/86150470/137071216-8718134f-a466-4d8c-806a-78d347c8a1bd.png">
<img width="698" alt="스크린샷 2021-10-13 오후 2 09 27" src="https://user-images.githubusercontent.com/86150470/137071224-132adfce-5d74-4caf-961c-941ea6296c6b.png">
<img width="702" alt="스크린샷 2021-10-13 오후 2 09 35" src="https://user-images.githubusercontent.com/86150470/137071227-e5902481-a8c4-43e4-9295-4d609642d57b.png">
<img width="722" alt="스크린샷 2021-10-13 오후 2 09 41" src="https://user-images.githubusercontent.com/86150470/137071237-9d56709a-942a-40c3-811e-993e7841da84.png">
