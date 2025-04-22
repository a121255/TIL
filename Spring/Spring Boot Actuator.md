# 📌 Spring Boot Actuator

## 📅 학습 날짜  
2025.04.23

## 📖 오늘 학습한 내용 요약
- Actuator란?
- 기본 사용법

## 🧠 정리
### 🔹 Actuator란?
Spring Boot Actuator는 애플리케이션 모니터링과 관리를 위한 도구
* 애플리케이션 상태 모니터링: 시스템이 정상적으로 작동하는지 실시간으로 확인
* 시스템 메트릭 수집: CPU, 메모리 사용량, HTTP 요청 수, 응답 시간 등 다양한 성능 지표를 수집
* 운영 중 문제 진단: 애플리케이션 상태와 로그 레벨을 실시간으로 확인하고 조정하여 문제 해결에 활용
* 외부 모니터링 시스템 연동: Prometheus, Grafana 같은 모니터링 도구와 연동하여 종합적인 모니터링 환경을 구축
* 애플리케이션 구성 정보 확인: 현재 적용된 환경 설정, 빈 정보, 매핑된 엔드포인트 등을 확
* 헬스 체크 자동화: 로드 밸런서나 컨테이너 오케스트레이션 도구가 서비스 상태를 확인하는 데 활용

### 🔹 사용법
1. 의존성 추가
   ```java
   implementation 'org.springframework.boot:spring-boot-starter-actuator'
   ```

2. 기본 설정 (application.yml)
   ```java
   management:
        endpoints:
          web:
            exposure:
              include: health,info,metrics,prometheus,loggers
        endpoint:
          health:
            show-details: always
   ```
    
<br>

3. 주요 엔드포인트
  기본적으로 /actuator 경로에서 다음 엔드포인트를 사용할 수 있다.
  * /actuator/health: 애플리케이션 상태 확인
  * /actuator/info: 애플리케이션 정보
  * /actuator/metrics: 메트릭 정보
  * /actuator/env: 환경 변수
  * /actuator/loggers: 로깅 레벨 관리
  * /actuator/httptrace: HTTP 요청 추적 (Spring Boot 2.2+ 에서는 추가 설정 필요)
  * /actuator/mappings: 모든 엔드포인트 매핑 정보
    
<br>

4. 보안 설정
  Actuator 엔드포인트는 민감한 정보를 노출할 수 있으므로 보안 설정이 중요하다.
     ```java
     @Configuration
     public class ActuatorSecurityConfig extends WebSecurityConfigurerAdapter {
         @Override
         protected void configure(HttpSecurity http) throws Exception {
             http.requestMatcher(EndpointRequest.toAnyEndpoint())
                 .authorizeRequests()
                 .requestMatchers(EndpointRequest.to("health", "info"))
                 .permitAll()
                 .anyRequest()
                 .hasRole("ACTUATOR_ADMIN")
                 .and()
                 .httpBasic();
         }
     }
      ```


## ❗ 깨달은 점 / 메모
Prometheus + Grafana로 모니터링 환경울 구축해봐야겠다.

## 🔗 참고 자료
https://incheol-jung.gitbook.io/docs/study/srping-in-action-5th/chap-16.
https://junuuu.tistory.com/968
