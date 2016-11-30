# Query Off Loading

출처 bcho.tistory.com/670

DB의 성능 향상을 위한 기법
DB 사용 시  Read, Write 대비 7:3 ~ 9:1이라는 개념을 바탕으로
Read DB와 Write DB를 분리하여 별도로 관리하고
Application에서도 DB 호출 시 Read Connection Pool과
Write Connection Pool을 별도로 관리한다.

Write DB가 Master DB가 되고
Read DB가 Slave DB가된다.
Master DB와 Slave DB 중간에 Master - Slave 간
복제를 위한 Staging DB를 두어 복제 시 발생할 수 있는
성능 저하 문제를 보완한다.

Master -> Staging -> Slave
형태로 데이터가 복제된다.

데이터 복제 시에는 CDC(Change Data Capture) 기술이 이용된다.
