# 📢 Redirect VS Forward

### 1. forward
``` 
             ↗  request → [URL1] ↘
   [클라이언트]                    [forward]
             ↖ response ← [URL2] ↙  //요청 정보가 그대로 유지
```
   <br>
- 클라이언트에서는 최초 호출한 URL만 표시 (서버에서 처리 해줘서),
  이동한 페이지의 URL정보는 볼 수 없다. <br>
  -> 그렇기 때문에 실제 웹 브라우저에서는 다른 페이지로 이동했는지 알 수 없음 <br>
- 동일한 웹 컨테이너에 있는 페이지로만 이동이 가능하다. <br>
- 현재 실행중인 페이지와 forward에 의해 호출 될 페이지는 request, response 객체를 공유한다. <br>

총정리: forward방식은 다음으로 이동한 URL로 요청정보를 그대로 전달한다. 말 그대로 forward(건네주다)하는 것이다. 그렇기에 사용자가 최초로 요청한 정보는 다음 URL에서도 유효하다.
<br>
<br>

### 2. redirect
``` 
            ↗  request1 → [ URL ]  
            ↙  redirect ← [  1  ]
   [클라이언트]       
            ↘  request2 → [ URL ]           
            ↖  response ← [  2  ]  //전혀 새로운 요청 수행
```
   <br>
- 웹 컨테이너는 redirect 명령이 들어보면 웹 브라우저에서 다른 페이지로 이동하라는 명령을 내린다. <br>
  -> 웹 브라우저는 URL을 지시된 주소로 바꾸고 그 주소로 이동한다. <br>
- 다른 웹컨테이너에 있는 주소로 이동이 가능하다. <br>
- 새로운페이지에서는 request, response 객체가 새롭게 생성한다. <br>

총정리: redirect방식은 최초요청을 받은 URL1에서 클라이언트에 redirect할 URL2를 리턴하고, 클라이언트에게 전혀 새로운 요청을 생성하여 URL2에 다시 요청을 보낸다. 그러므로 최초 요청정보는 유효하지 않는다.


<table>
	<tr>
		<th></th>
        <th>forward</th>
        <th>redirect</th>
	</tr>
	<tr>
		<td>URL 변화여부</td>
        <td>X</td>
        <td>O</td>
	</tr>
    <tr>
		<td>객체의 재사용여부</td>
        <td>O</td>
        <td>X</td>
	</tr>
    <tr>
		<td>사용용도</td>
        <td>시스템(session, DB)에 변화가 생기지
        않는 단순조회(리스트보기, 검색 등)</td>
        <td>시스템(session, DB)에 변화가 생기는
        요청(로그인, 회원가입, 글쓰기 등)</td>
	</tr>
</table>

<br>
<br>


 