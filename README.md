# Skills Marketplace

팀 공유 Claude Code 스킬 저장소

## 스킬 목록

| 스킬 | 설명 | 연결 방법 |
|------|------|----------|
| [notion-lookup](notion-lookup/) | Notion 강의 대시보드 조회 | MCP 서버 연결 (아래 참조) |

## 설치

### notion-lookup

```bash
claude mcp add --transport http notion-lecture https://mcp-notion-lookup-production.up.railway.app/mcp
```

연결 후 Claude Code에서 `lecture_search`, `lecture_detail`, `lecture_proposals` 도구를 사용할 수 있습니다.
