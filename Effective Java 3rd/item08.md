# finalizer와 cleaner 사용을 피하라
- 실행시점 예측불가, 느리며, 일반적으로 불필요하므로 사용X
    - finalizer 공격에도 노출되어 보안적으로 문제
- 대안 : AutoCloseable
    - try-with-resources (item09)
- 사용처 (이마저도 심사숙고가 필요)
    - 자원 소유자가 close 호출하지 않은 경우를 위한 안전망
    - 중요하지 않은 네이티브 자원 회수용