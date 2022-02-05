### `DTO(Data Transfer Object)` 란? 
- 데이터 전달용
- 데이터를 전달하기 위해 사용하는 객체
- 데이터를 담아서 전달하는 바구니
- 메서드는 only getter/setter만 갖음
- 다른 로직은 갖지 않는다(순수하게 데이터 전달 만을 위한 객체이기 때문) <br/>
  ㄴ 보내는 쪽에서 setter를 사용해 데이터를 DTO에 담아보내고 받는곳에서 getter를 사용해 전달받은 DTO로부터 데이터를 꺼내는 방식이다.
- ex) 시험 볼 떄 OMR카드라고 생각, 답을 하나 다르게 쓰면 결과 값이 다룸
<br/>
- 생성자를 사용하고 setter를 빼면 불변객체가 됨 그러면 데이터가 변조되지않음

### `VO(Value Object)` 란? 
- 값 표현용 (값으로만 비교가 됨)
  ex)  지폐의 경우 같은 만원이지만 고유번호가 다름(고유번호를 각 객제의 주소라고 생각) -> 하지만 다같은 만원임
- VO는 값 자체를 표현하기 때문에 불변객체이다
- 따라서 setter 성격의 메서드는 포함X, 생성자를 만들어야함
- getter/setter 외에 다른 로직 포함0
- equals메서드, hashCode 메서드 모두 오버라이딩 해야함 <br/>
  ㄴ hashCode() 리턴값 -> (같음) -> equals() 리턴값 -> (true) -> 동등객체

### `DTO vs VO`
<table>
	<tr>
		<th></th>
        <th>DTO</th>
        <th>VO</th>
	</tr>
	<tr>
		<td>용도</td>
        <td>데이터전달</td>
        <td>값자체표현</td>
	</tr>
    <tr>
		<td>동등결정</td>
        <td>속성값이 모두 같다고 해서 같은 객체X</td>
        <td>속성값이 모두 같으면 같은 객체O</td>
	</tr>
    <tr>
		<td>가변/불변</td>
        <td>setter 존재 시 가변, setter 비존재시 불변</td>
        <td>불변</td>
	</tr>
    <tr>
		<td>로직</td>
        <td>only g/s</td>
        <td>g/s외 로직 가질수 있음</td>
	</tr>
</table>



 


