#gsecu_v0.4

1. 개요.
	gsecu_v0.4는 사용자 데이터 설정(Users.java)을 이용하지 않고, 
	DB를 조회하여 인증하도록 커스터마이징 한다.

2. 목표.
	1) 로그인 시 DB를 조회하여 사용자 ID, PASSWORD를 가져와 
	사용자가 인증화면에서 입력한 ID, PASSWORD와 비교하여 인증하도록 커스터마이징. 
      
3. 주요내용.
	DB를 조회하기 위해 인증 관련 DB설계를 한다.
	
	DB연동을 위해 pom.xml에 JPA 라이브러리를 추가한다.
	
	Spring Security에서 DB연동을 위한 커스터마이징을 하려면 우선 눈여겨 봐야할 것은
	"UserDetails", "UserDetailsService", "GrantedAuthority" 세가지 인터페이스이다.
	즉, 이 세가지 인터페이스를 구현하는 클래스를 작성해야 하는 것이다.
	
	
	3.1. Users 클래스를 DB로 전환.
		: v0.3에서 Users 클래스로 만들어져 있던 사용자 인증 데이터를
		이제 DB를 조회해서 불러와야한다.
		그를 위해 DB에 사용자, 권한 데이터를 저장하는 테이블을 설계한다.(1)
		테이블 설계 후, 이 데이터를 조회하는 엔티티 클래스를 작성한다.(2)
		

	3.2. DB 설계.
		1) 사용자 Table : USER
			-- 사용자
			CREATE TABLE USER (
				user_id  BIGINT UNSIGNED NOT NULL, -- 사용자ID
				username VARCHAR(30)     NOT NULL, -- 아이디
				passwd   VARCHAR(100)    NOT NULL  -- 비번
				nickname VARCHAR(30)     NOT NULL  -- 닉네임
			);
			
			-- 사용자 기본키
			CREATE UNIQUE INDEX PK_USER
				ON USER ( -- 사용자
					user_id ASC -- 사용자ID
				);
			
			-- 사용자
			ALTER TABLE USER
				ADD
					CONSTRAINT PK_USER -- 사용자 기본키
					PRIMARY KEY (
						user_id -- 사용자ID
					);
		
		2) 권한 Table : AUTHORITY
			-- 권한
			CREATE TABLE AUTHORITY (
				auth_id   BIGINT UNSIGNED NOT NULL, -- 권한ID
				auth_code VARCHAR(30)     NOT NULL, -- 권한코드
				auth_name VARCHAR(100)    NOT NULL, -- 권한이름
				user_id   BIGINT UNSIGNED NOT NULL  -- 사용자ID
			);
			
			-- 권한 기본키
			CREATE UNIQUE INDEX PK_AUTHORITY
				ON AUTHORITY ( -- 권한
					auth_id ASC -- 권한ID
				);
			
			-- 권한
			ALTER TABLE AUTHORITY
				ADD
					CONSTRAINT PK_AUTHORITY -- 권한 기본키
					PRIMARY KEY (
						auth_id -- 권한ID
					);
			
			-- 권한
			ALTER TABLE AUTHORITY
				ADD
					CONSTRAINT FK_USER_TO_AUTHORITY -- 사용자 -> 권한
					FOREIGN KEY (
						user_id -- 사용자ID
					)
					REFERENCES USER ( -- 사용자
						user_id -- 사용자ID
					)
					ON DELETE NO ACTION
					ON UPDATE NO ACTION;
		
		3) 고유번호 Table : ID_SEQUENCES
			-- ID시퀀스
			CREATE TABLE ID_SEQUENCES (
				id_type  VARCHAR(50)     NOT NULL, -- id타입
				next_val BIGINT UNSIGNED NOT NULL  -- 다음값
			);
			
			-- ID시퀀스 기본키
			CREATE UNIQUE INDEX PK_ID_SEQUENCES
				ON ID_SEQUENCES ( -- ID시퀀스
					id_type ASC -- id타입
				);
			
			-- ID시퀀스
			ALTER TABLE ID_SEQUENCES
				ADD
					CONSTRAINT PK_ID_SEQUENCES -- ID시퀀스 기본키
					PRIMARY KEY (
						id_type -- id타입
					);
			
			insert into ID_SEQUENCES values ('BASE', 1000);

	3.3. 클래스 설명.
		1) net.gongple.gsecu.domain.User
			: 사용자 엔티티
			
		2) net.gongple.gsecu.domain.Authority
			: 권한 엔티티
		
4. 추가내용.
	4.1. 
		1) 
			: 
		
		2) 
			: 
			
	4.2. 
		1) 
