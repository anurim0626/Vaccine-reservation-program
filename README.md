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
![image](https://user-images.githubusercontent.com/102803326/201582858-51d2048a-17dd-4890-94c2-b51789d745af.png)

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
![image](https://user-images.githubusercontent.com/102803326/201582909-c1c6ef2d-ae7e-4fa4-8cc9-45d7fb267ef6.png)
#### vaccine_list.jsp(예약리스트페이지)
![image](https://user-images.githubusercontent.com/102803326/201582934-e31f0969-4bd4-41aa-a764-c9e6fa60856f.png)
#### vaccine_search_list.jsp(예약조회결과 페이지)
![image](https://user-images.githubusercontent.com/102803326/201582958-40347dcc-2be1-4ab1-8f70-e58866a6b5d0.png)
#### vaccine_total.jsp(백신예약현황 페이지)
![image](https://user-images.githubusercontent.com/102803326/201582984-963b7a01-cd52-4035-ba0c-f1d7623c8029.png)
