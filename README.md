# TeamSparta Skills Marketplace

팀스파르타 공유 Claude Code 스킬 마켓플레이스

## 설치

```bash
# 마켓플레이스 등록
/plugin marketplace add teamsparta-biz/skills-marketplace

# 플러그인 설치
/plugin install notion-lookup@teamsparta-skills
```

## 사전 설정

### notion-lookup

MCP 서버 연결이 필요합니다:

```bash
claude mcp add --transport http notion-lecture https://mcp-notion-lookup-production.up.railway.app/mcp
```

## 스킬 목록

| 스킬 | 설명 | 버전 |
|------|------|------|
| [notion-lookup](skills/notion-lookup/) | Notion 강의 대시보드 조회 | 2.0.0 |
