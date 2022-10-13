# QueryDSL

QueryDSL란, HQL (Hibernate Query Language) 쿼리를 타입에 안전하게 생성 및 관리할 수 있는 프레임워크이다. 쉽게 말하면 쿼리를 작성할 수 있게 해주는 프레임워크라고 생각하면 된다.

## QueryDSL 장점
- 동적 쿼리 (QueryDSL의 가장 큰 장점)
- 컴파일 시점에 문법 오류를 발견할 수 있다.
- 코드 자동완성(IDE 도움)으로 단순하고 쉽다.
- 코드 모양이 JPQL과 유사하다.

## QueryDSL을 사용하는 이유

하나의 문장을 3가지로 표현할 수 있다.
1. JPA
```
List<Article> findByUserId(String userId);
```
2. JPA의 Native Query
```
@Query(value = “SELECT * FROM board WHERE user_id IN (SELECT id FROM user WHERE isActive = :isActive)”, nativeQuery = true)
List<Article> findByUserState(boolean isActive);
```

3. QueryDSL
```
private JPAQueryFactory queryFactory;

Public List<Article> findByUserState(boolean isActive) {
	return queryFactory.selectFrom(board)
		.where(
            board.userId.in(
                JPAExpressions
                    .select(user.id)
                    .from(user)
                    .where(user.isActive.eq(isActive))
            )
        )
        .fetch();
}
```
```
public List<StudyRoom> search(String name, Boolean isPublic, String category,	Pageable pageable) {
	return queryFactory.selectFrom(studyRoom)
	  .where(containsName(name),
	        eqIsPublic(isPublic),
	        eqCategory(category),
	        notEnd())
	  .orderBy(studyRoomSort(pageable))
	  .offset(pageable.getOffset())
	  .limit(pageable.getPageSize()).fetch();
}
```

```
public User(String name) {
	return queryFactory
            .selectFrom(member)
            .join(user.team, team).fetchJoin()
            .where(member.username.eq(name))
            .fetchOne();
}
```

QueryDSL은 JPA보다 좀 더 세세한 조건 설정을 할 수 있으며, JPA의 Native Query보다 가독성이 좋다. 쿼리 파라미터 타입까지 확인할 수 있다.

## QueryDSL 설정

build.gradle
```
configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

dependencies {
	//queryDSL
	implementation 'com.querydsl:querydsl-jpa'
	annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"
}
```

QueryDSLConfig
```
import com.querydsl.jpa.impl.JPAQueryFactory;	
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
	
@EnableJpaAuditing
@Configuration
public class QuerydslConfig {
	
  @PersistenceContext
	private EntityManager entityManager;
	
	@Bean
	public JPAQueryFactory jpaQueryFactory() {
	  return new JPAQueryFactory(entityManager);
	}
}
```

