# r2dbc + webflux 조합의 성능을 mybatis와 비교해보자

https://spring.io/guides/gs/accessing-data-r2dbc 에서 제공하는 샘플에 Mybatis와 Taurus 부하 테스트 설정을 추가.

## 환경

* Spring Boot 2.4.2
* r2dbc 
* Mybatis 2.1.4 
* H2 database
* Taurus BlazeMeter

## Reactive System

* Backend, Frontend 양쪽 다 Reactive 적용을 해야 의미가 있음
* 복잡한 MSA 아키텍처에서 유용함

## 결과(정리중)
	
~~~~
[concurrency: 500]
┌───────────────┬────────┬─────────┬────────┬───────────────────────────────────────────┐
│ label         │ status │    succ │ avg_rt │ error                                     │
├───────────────┼────────┼─────────┼────────┼───────────────────────────────────────────┤
│ /r2/customers │  FAIL  │ 100.00% │  0.122 │ Non HTTP response message: Read timed out │
└───────────────┴────────┴─────────┴────────┴───────────────────────────────────────────┘
┌───────────────┬────────┬─────────┬────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ label         │ status │    succ │ avg_rt │ error                                                                                                │
├───────────────┼────────┼─────────┼────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ /mb/customers │  FAIL  │ 100.00% │  0.082 │ Non HTTP response message: Connect to localhost:8080 [localhost/127.0.0.1] failed: connect timed out │
│               │        │         │        │ Non HTTP response message: Connection reset                                                          │
└───────────────┴────────┴─────────┴────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────┘

[concurrency : 1000]
┌───────────────┬────────┬─────────┬────────┬───────────────────────────────────────────┐
│ label         │ status │    succ │ avg_rt │ error                                     │
├───────────────┼────────┼─────────┼────────┼───────────────────────────────────────────┤
│ /r2/customers │  FAIL  │ 100.00% │  0.245 │ Non HTTP response message: Read timed out │
└───────────────┴────────┴─────────┴────────┴───────────────────────────────────────────┘
┌───────────────┬────────┬────────┬────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ label         │ status │   succ │ avg_rt │ error                                                                                                │
├───────────────┼────────┼────────┼────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ /mb/customers │  FAIL  │ 99.96% │  0.146 │ Non HTTP response message: Connect to localhost:8080 [localhost/127.0.0.1] failed: connect timed out │
└───────────────┴────────┴────────┴────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────┘

[concurrency : 2000]
┌───────────────┬────────┬─────────┬────────┬───────────────────────────────────────────┐
│ label         │ status │    succ │ avg_rt │ error                                     │
├───────────────┼────────┼─────────┼────────┼───────────────────────────────────────────┤
│ /r2/customers │  FAIL  │ 100.00% │  0.494 │ Non HTTP response message: Read timed out │
└───────────────┴────────┴─────────┴────────┴───────────────────────────────────────────┘
┌───────────────┬────────┬────────┬────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ label         │ status │   succ │ avg_rt │ error                                                                                                │
├───────────────┼────────┼────────┼────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ /mb/customers │  FAIL  │ 99.65% │  0.259 │ Non HTTP response message: Connect to localhost:8080 [localhost/127.0.0.1] failed: connect timed out │
└───────────────┴────────┴────────┴────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────┘
~~~~

## 기타

* h2 database 콘솔 : http://localhost:8080/h2-console