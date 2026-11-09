# 1. Ubuntu Linux 베이스 이미지 사용
FROM ubuntu:24.04

# 3. 환경변수 설정
ENV APP_HOME=/app
ENV APP_PORT=8080

# 4. 필요한 패키지 설치
# python3: 간단한 웹 서버 실행용
# curl: 헬스체크용

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends python3 curl ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# 5. 애플리케이션 전용 그룹 및 사용자 생성
RUN groupadd -r appgroup && \
    useradd -r -g appgroup -d ${APP_HOME} -s /usr/sbin/nologin appuser

# 6. 작업 디렉터리 설정
WORKDIR ${APP_HOME}

# 7. 정적 파일 복사
COPY index.html ${APP_HOME}/index.html

# 8. 파일 권한 변경
RUN chown -R appuser:appgroup ${APP_HOME}

# 9. root가 아닌 사용자로 실행
USER appuser

# 10. 컨테이너에서 사용할 포트 명시
EXPOSE 8080

# 11. 헬스체크 설정
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -fsS http://localhost:${APP_PORT}/ || exit 1

# 12. 컨테이너 시작 시 실행할 명령
CMD ["sh", "-c", "python3 -m http.server ${APP_PORT} --bind 0.0.0.0"]

