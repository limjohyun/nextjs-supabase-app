# 개발 도구 세팅 로드맵

`nextjs-supabase-app`(Next.js 16 App Router + TypeScript + Tailwind v3 + Supabase, 1인 프로젝트)에 커밋 전 자동 포맷/린트 검증과 최소한의 CI 안전망을 갖추기 위한 작업 순서. 1인/소규모 프로젝트 규모에 맞게 과설계하지 않는 것을 원칙으로 한다.

상세 배경, 확인된 현재 상태, 각 단계의 세부 코드/설정 값은 [`.claude/plans/ultraplan-approved-executing-linear-wilkes.md`](.claude/plans/ultraplan-approved-executing-linear-wilkes.md) 참고.

## 확정 사항

- CI(GitHub Actions, lint + typecheck + build) **포함**
- commitlint **제외**
- 테스트 프레임워크(Vitest 등) **제외** (별도 후속 결정)
- Prettier: 더블쿼트 + 세미콜론 + `printWidth: 80`
- `git push` / PR 생성은 이번 로드맵 실행 범위에 포함하지 않음 — 로컬 검증까지 마친 뒤 별도 확인 후 진행

## 단계

- [x] **1. 줄바꿈 정규화** — `.editorconfig`, `.gitattributes`(`* text=auto eol=lf`) 생성
- [x] **2. ESLint 버전 정합화** — `eslint-config-next@^16.2.11` 설치, `npm run lint` 확인 (flat config 직접 export 방식으로 전환되어 `eslint.config.mjs`를 `FlatCompat` 대신 `eslint-config-next/core-web-vitals` · `eslint-config-next/typescript` 직접 import로 변경, 신규 규칙 위반 2건 수정)
- [x] **3. Prettier 도입** — `prettier` / `prettier-plugin-tailwindcss` / `eslint-config-prettier` 설치, `.prettierrc.json` / `.prettierignore` 생성, `eslint.config.mjs`에 반영, `format` / `format:check` 스크립트 추가 후 전체 재포맷은 별도 커밋(`chore: apply prettier formatting`)으로 분리
- [x] **4. Husky + lint-staged** — 설치, `"prepare": "husky"`, `.husky/pre-commit`에 `npx lint-staged`, `package.json`에 lint-staged 설정 추가
- [x] **5. 타입체크 스크립트** — `"typecheck": "tsc --noEmit"` 추가
- [x] **6. GitHub Actions CI** — `.github/workflows/ci.yml`: push/PR(main) 트리거, Node 20, `npm ci → lint → typecheck → build`
- [x] **7. 에디터 설정** — `.vscode/settings.json`(저장 시 자동 포맷/ESLint fix, Tailwind IntelliSense `classRegex`), `.vscode/extensions.json`(prettier-vscode / vscode-eslint / vscode-tailwindcss)
- [x] **8. CLAUDE.md 정합화** — `## Commands`에 `format` / `format:check` / `typecheck` 3줄 추가

## 건드리지 않는 것

- `proxy.ts` / `lib/supabase/proxy.ts`의 함수명·구조
- `next`, `@supabase/*`의 `"latest"` 버전 표기
- 테스트 프레임워크, commitlint

## 검증

1. `npm run lint` 통과
2. `npm run typecheck` 통과
3. `npm run format:check` 통과 (전체 재포맷 커밋 이후 diff 없어야 함)
4. `npm run build` 통과
5. 더미 파일로 포맷 깨뜨린 뒤 커밋 → pre-commit 훅이 자동 수정하는지 확인 후 되돌림
6. (사용자 확인 후) push하여 GitHub Actions 성공 확인

## 완료 후 저장할 체크리스트 (메모리)

- [ ] `npm run lint` 통과
- [ ] `npm run typecheck` 통과
- [ ] `npm run format:check` 통과
- [ ] `npm run build` 통과
- [ ] Husky pre-commit 훅 실동작 확인 (더미 커밋)
- [ ] CI(GitHub Actions) push 후 초록불 (push는 사용자 확인 후에만)
