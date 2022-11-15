# Vaccine-reservation-program
### 1) DB분석
#### -3개의 테이블 생성한다
#### 주민정보 테이블에서 주어진 데이터는 insert into values 쿼리문으로 입력한다.
![image](https://user-images.githubusercontent.com/102803326/201582727-a52758d4-4d81-4bec-92da-e37502431651.png)
#### 주민정보셈플데이터
![image](https://user-images.githubusercontent.com/102803326/201582757-f1a73b44-1d7d-4449-9b07-7905179471f5.png)
#### 병원정보 테이블에서 주어진 데이터는 insert into values 쿼리문으로 입력한다.
![image](https://user-images.githubusercontent.com/102803326/201582776-4037237e-1867-4cbe-bb3e-4407824ca922.png)
#### 병원정보 셈플
![image](https://user-images.githubusercontent.com/102803326/201582803-ffb784cb-b4b8-48a6-a080-016e266d7060.png)
#### 백신예약정보 테이블에서 주어진 데이터는 insert into values 쿼리문으로 입력하고 추가데이터는 폼화면(백신예약 페이지)에서 직접입력한다.
![image](https://user-images.githubusercontent.com/102803326/201582818-444d32ad-228e-4d6c-9405-0b667d96fe20.png)
![image](https://user-images.githubusercontent.com/102803326/201582829-c82935bd-aef3-431c-a295-9463ebeb587b.png)
### 2) 페이지 분석

#### vaccine_reservation.jsp(백신예약 페이지)
![image](https://user-images.githubusercontent.com/102803326/201843764-a09524a2-cb01-433b-9a38-4205a926c863.png)

```
  <%@page import="java.text.SimpleDateFormat"%>
  <%@page import="java.util.Date"%>
  <%@page import="java.sql.PreparedStatement"%>
  <%@page import="DBPKG.DBConnection"%>
  <%@page import="java.sql.Connection"%>
  <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  <%
    String sql = "INSERT INTO TBL_VACCRESV_202108 VALUES(?,?,?,?,?,?)";
    
    Connection conn = DBConnection.getConnection();
    PreparedStatement ps = conn.prepareStatement(sql);
    
    request.setCharacterEncoding("UTF-8");
    
    ps.setString(1, request.getParameter("rNum"));
    ps.setString(2, request.getParameter("jNum"));
    ps.setString(3, request.getParameter("hCode"));
    ps.setString(4, request.getParameter("rDate"));
    ps.setString(5, request.getParameter("rTime"));
    ps.setString(6, request.getParameter("vCode"));
    
    ps.executeUpdate();
    
  %>
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  <link type="text/css" rel="stylesheet" href="css/style.css">
  </head>
  <body>
    <jsp:forward page="index.jsp"></jsp:forward>
  </body>
  </html> 
```

#### ① (변형) 예약번호는 자동으로 생성하며, 수정이 불가하도록 처리함
#### ② (변형) 백신코드는 셀렉트 박스로 만들며 값은 V001, V002, V003으로 처리하되 눈에 보이는 것은 V001은 A백신, V002는 B백신, V003은 C백신으로 표시함
#### ③ (변형) 라디오버튼으로 처리하되 값은 H001, H002, H003, H004 이나 눈에 보이는 것은 가_병원, 나_병원, 다_병원, 라_병원으로 표시함. 
#### ④ 예약날짜는 YYYYMMDD로 작성하며, 예약시간은 0930, 1130처럼 4자리 숫자로 나타내고 24시간제로 표현함.
#### ⑤ 모든 항목에 대하여 유효성 검사를 실시하며, 값이 비어있거나 선택되지 않은 경우에는 오류메세지를 띄운 후 포커스를 이동함.

#### vaccine_search.jsp(예약조회페이지)
![image](https://user-images.githubusercontent.com/102803326/201844118-2e3f5389-7af5-4e27-bca7-072a6e635be2.png)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link type="text/css" rel="stylesheet" href="css/style.css">
<script type="text/javascript">
	function checkValue2() {
		if(!document.sData.rNum.value) {
			alert("회원번호를 입력하세요.");
			sData.rNum.focus();
			return false;
		} 
		return true;}
</script>
</head>
<body>
	<header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
 </header>

 <nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
 </nav>
		
 <section>
	<div class="section">
		<div class="title">백신예약조회</div>
		<div>
		<form name="sData" action="search_reservation_table.jsp" method="post" onsubmit="return checkValue2()">
			<table class="table_list">
				<tr>
					<th>예약번호</th>
					<td><input type="text" name="rNum"></td>
				</tr>
				<tr>
					<th colspan="2">
						<input class="btn_st" type="submit" value="조회하기">
						<input class="btn_st" type="button" value="홈으로" onclick="location.href='index.jsp'">
					</th>
				</tr>
			</table>
		</form>
		</div>
	</div>
 </section>	
 <footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
 </footer>	
</body>
</html>
```
#### 1 예약번호를 넣고 조회하는 페이지 생성
#### 2 예약번호에 해당하는 주민이 있는 경우는 아래와 같이 나타냄.
#### vaccine_list.jsp(예약페이지)
![image](https://user-images.githubusercontent.com/102803326/201859733-bb81c021-e6e5-46de-8e8f-4111a68e82cc.png)
```jsp
<%@page import="DBPKG.DBConnection"%>
<%@page import="java.sql.*"%>

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<% String sql="select resvno, jumin, vcode, hospcode, resvdate, resvtime "
		     +"from TBL_VACCRESV_202108 order by resvno";

   Connection conn = DBConnection.getConnection();
   PreparedStatement ps = conn.prepareStatement(sql);
   ResultSet rs= ps.executeQuery();
%>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link type="text/css" rel="stylesheet" href="css/style.css">
</head>
<body>
 <header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
 </header>

 <nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
 </nav>
		
	<section><div class="section">
		<div class="title">예악정보리스트</div>
		<div class="scroll"><table class="table_list">
		 <tr><th>예약번호</th><th>주민번호</th><th>백신코드</th><th>병원코드</th><th>예약날짜</th><th>예약시간</th></tr>
		
		<% while(rs.next()){%>
		<tr>
		  <td><%= rs.getString(1) %></td>
		  <td><%= rs.getString(2) %></td>
		  <td><%= rs.getString(3) %></td>
		  <td><%= rs.getString(4) %></td>
		  <td><%= rs.getString(5) %></td>
		  <td><%= rs.getString(6) %></td>
		
		<%} %>
		</tr></table></div>
	</div></section>
 <footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
 </footer>
</body>
</html>
```
1. 주민등록번호를 이용하여 성별을 나타냄.(주민등록번호 뒷 첫 째자리가 1이면 남, 2면 여)
2. 예약날짜는 YYYY년MM월DD일의 형태로 출력합.
3. 예약시간은 hh:mm의 형태로 출력함.
4. 병원주소는 10은 서울, 20은 대전, 30은 대구, 40은 광주로 출력함.
5. 들어가기 버튼 클릭시 백신예약 조회페이지로 이동함.
#### vaccine_search_list.jsp(예약리스트 페이지)
![image](https://user-images.githubusercontent.com/102803326/201582958-40347dcc-2be1-4ab1-8f70-e58866a6b5d0.png)
```jsp
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="DBPKG.DBConnection"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%
    StringBuffer sb = new StringBuffer();
    
    sb.append(" select h.HOSPADDR,                             ")
    .append(" case h.HOSPADDR                                  ")
    .append(" 	when '10' then '서울'                           ")
    .append(" 	when '20' then '대전'                           ")
    .append(" 	when '30' then '대구'                           ")
    .append(" 	when '40' then '광주'                           ")
    .append(" end as HOSPAREA,                                 ")
    .append(" count(v.HOSPCODE)                                ")
    .append(" from TBL_VACCRESV_202108 v, TBL_HOSP_202108 h    ")
    .append(" where h.HOSPCODE = v.HOSPCODE(+)               ")//  오른쪽 테이블에 null값을 허용하는 외부조인
    .append(" group by HOSPADDR                                ")
    .append(" order by HOSPADDR                                ");
    
    String sql = sb.toString();
    
    Connection conn = DBConnection.getConnection();
    PreparedStatement ps = conn.prepareStatement(sql);
    ResultSet rs = ps.executeQuery();
    
    int sum = 0;
    %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link type="text/css" rel="stylesheet" href="css/style.css">
</head>
<body>
 <header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
 </header>

 <nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
 </nav>
		
 <section>
	<div class="section">
		<div class="title">백신예약현황</div>
		<div>
			<table class="table_list align_center">
				<tr>
					<th>병원지역</th>
					<th>병원지역명</th>
					<th>접종예약건수</th>
				</tr>
				<%while(rs.next()){ %>
				<tr>
					<td><%=rs.getString(1) %></td>
					<td><%=rs.getString(2) %></td>
					<td><%=rs.getString(3) %></td>
				</tr>
				<%
				//sum = sum + rs.getString(3);
				sum += Integer.parseInt(rs.getString(3));
				} %>
				<tr>
					<td colspan="2">총합</td>
					<td><%=sum %></td>
				</tr>
			</table>
		</div>
	</div>
 </section>	
 <footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
 </footer>
</body>
</html>
```
#### vaccine_total.jsp(백신예약현황 페이지)
![image](https://user-images.githubusercontent.com/102803326/201860575-d0676151-02d8-4300-b337-4f4ca6c45308.png)
```jsp
<%@page import="DBPKG.DBConnection"%>
<%@page import="java.sql.*"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%
    
   
    String sql =" select max(RESVNO) from TBL_VACCRESV_202108 " ;                                                  
    
    Connection conn = DBConnection.getConnection();
    PreparedStatement ps = conn.prepareStatement(sql);
    ResultSet rs = ps.executeQuery();
    
    rs.next();
	int num = rs.getInt(1)+1;
        
    %>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link type="text/css" rel="stylesheet" href="css/style.css">
<script type="text/javascript">
	function remove() {
		alert("정보를 지우고 다시 입력합니다.");
		document.rData.reset();
		rData.rNum.focus();
	}
	
	function checkVal(){
		if(!document.rData.rNum.value){
			alert("예약번호가 입력되지 않았습니다.");
			rData.rNum.focus();
			return false;
		}else if(!document.rData.jNum.value){
			alert("주민번호가 입력되지 않았습니다.");
			rData.jNum.focus();
			return false;
		}
		else if(!document.rData.vCode.value){
			alert("백신코드가 입력되지 않았습니다.");
					return false;
		}
		else if(!document.rData.hCode.value){
			alert("병원코드가 입력되지 않았습니다.");
					return false;
		}
		else if(!document.rData.rDate.value){
			alert("예약날짜가 입력되지 않았습니다.");
			rData.rDate.focus();
			return false;
		}
		else if(!document.rData.rTime.value){
			alert("예약시간이 입력되지 않았습니다.");
			rData.rTime.focus();
			return false;
		}
	}
	
</script>
</head>
<body>
	<header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
 </header>

 <nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
 </nav>
	<section>	<div class="section">
		<div class="title">백신예약</div>
		<div>
			<form name="rData" method="post" action="insert_reservation.jsp" onsubmit="return checkVal()">
			<table class="table_list">
				<tr>
					<th>예약번호</th>
					<td><input type="text" name="rNum" readonly="readonly" value="<%=num%>"> 예)20210011</td>
				</tr>
				<tr>
					<th>주민번호</th>
					<td><input type="text" name="jNum"> 예)790101-1111111</td>
				</tr>
				<tr>
					<th>백신코드</th>
					<td>
					<select name="vCode">
						<option value="">백신선택</option>
						<option value="V001">A백신</option>
						<option value="V002">B백신</option>
						<option value="V003">C백신</option>
					</select>
					 예)V001, V002, V003</td>
				</tr>
				<tr>
					<th>병원코드</th>
					<td>
					<label><input type="radio" name="hCode" value="H001">가_병원</label>
					<label><input type="radio" name="hCode" value="H002">나_병원</label>
					<label><input type="radio" name="hCode" value="H003">다_병원</label>
					<label><input type="radio" name="hCode" value="H004">라_병원</label>
					 예)H001, H002, H003, H004</td>
				</tr>
				<tr>
					<th>병원코드</th>
					<td>
					<label><input type="checkbox" name="hCode" value="H001">가_병원</label>
					<label><input type="checkbox" name="hCode" value="H002">나_병원</label>
					<label><input type="checkbox" name="hCode" value="H003">다_병원</label>
					<label><input type="checkbox" name="hCode" value="H004">라_병원</label>
					 예)H001, H002, H003, H004</td>
				</tr>
				<tr>
					<th>예약날짜</th>
					<td><input type="text" name="rDate"> 예)20210101</td>
				</tr>
				<tr>
					<th>예약시간</th>
					<td><input type="text" name="rTime"> 예)0930, 1130</td>
				</tr>
				<tr>
					<th colspan="2">
						<input type="submit" value="등록">
						<input type="button" value="취소" onclick="remove()">
					</th>
				</tr>
			</table>
			</form>
		</div>
	</div></section>
 <footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
</footer>	
</body>
</html>
```
1 병원이 있는 지역별로 접종예약건수의 합을 출력함.
2 모든 병원의 접종예약선수를 합쳐서 총합을 출력함. 
3 (변형) 기존 문제는 광주지역의 경우 예약건수가 없어 출력을 하지 않았으나, 예약이 없는 경우 0으로 표기하여 출
력함.
