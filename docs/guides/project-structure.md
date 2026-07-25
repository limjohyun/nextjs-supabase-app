# 프로젝트 구조 가이드

이 문서는 이 저장소(nextjs-supabase-app, Next.js 16 + Supabase)의 폴더 구조, 파일 조직 및 네이밍 컨벤션을 정의합니다.

## 🏗️ 전체 프로젝트 구조

이 프로젝트는 `src/` 없이 앱 루트에 바로 `app/`, `components/`, `lib/`가 위치합니다.

```
nextjs-supabase-app/
├── docs/                   # 📚 프로젝트 문서
│   └── guides/            # 개발 가이드 모음
├── app/                   # 🚀 Next.js App Router
├── components/            # 🧩 React 컴포넌트
├── lib/                   # 🛠️ 유틸리티 및 Supabase 클라이언트
├── proxy.ts               # 🔐 세션 갱신/라우트 보호 (middleware.ts 대체)
├── components.json        # shadcn/ui 설정
├── next.config.ts         # Next.js 설정
├── package.json           # 의존성 및 스크립트
├── tsconfig.json          # TypeScript 설정
└── CLAUDE.md              # 개발 지침 메인 문서
```

## 📁 세부 폴더 구조

### app/ - App Router 페이지

```
app/
├── layout.tsx            # 🎨 루트 레이아웃 (전역 설정)
├── page.tsx               # 🏠 홈페이지 (/)
├── globals.css             # 🎨 전역 CSS 스타일
├── favicon.ico             # 🔖 파비콘
├── instruments/            # 예제 Server Component 데이터 패칭 페이지
│   └── page.tsx
├── protected/              # 로그인 필요 영역
│   ├── layout.tsx
│   └── page.tsx
└── auth/                   # 인증 관련 페이지 (로그인/가입/비번 재설정 등)
    ├── login/page.tsx
    ├── sign-up/page.tsx
    ├── sign-up-success/page.tsx
    ├── forgot-password/page.tsx
    ├── update-password/page.tsx
    ├── error/page.tsx
    └── confirm/route.ts    # 이메일 인증 콜백 라우트 핸들러
```

**🚀 App Router 규칙:**

- `page.tsx`: 해당 경로의 메인 페이지
- `layout.tsx`: 레이아웃 컴포넌트 (자식 페이지 감쌈)
- `loading.tsx`: 로딩 UI (필요시)
- `error.tsx`: 에러 UI (필요시)
- `not-found.tsx`: 404 페이지 (필요시)

### components/ - 컴포넌트 조직

```
components/
├── ui/                     # 🎛️ shadcn/ui 기본 컴포넌트 (button, card, input, label, badge, checkbox, dropdown-menu)
├── tutorial/                # 스타터킷 온보딩 UI (env 미설정 시 노출되는 튜토리얼 스텝) — 앱 기능 아님
├── auth-button.tsx           # 로그인 상태에 따른 헤더 버튼
├── login-form.tsx            # 로그인 폼 (Client Component)
├── sign-up-form.tsx          # 회원가입 폼
├── forgot-password-form.tsx  # 비밀번호 찾기 폼
├── update-password-form.tsx  # 비밀번호 변경 폼
├── logout-button.tsx
├── theme-switcher.tsx        # next-themes 다크모드 토글
├── hero.tsx / next-logo.tsx / supabase-logo.tsx / deploy-button.tsx / env-var-warning.tsx  # 랜딩 페이지/스타터킷 잔여 UI
```

이 프로젝트는 컴포넌트를 `layout/`, `navigation/`, `sections/`, `providers/` 등으로 세분화하지 않습니다. `ui/`(shadcn 원자 컴포넌트)와 나머지(플랫 배치) 두 층만 존재합니다. 새 하위 폴더가 실제로 필요해지기 전까지는 만들지 마세요.

### lib/ - 유틸리티 및 Supabase 클라이언트

```
lib/
├── utils.ts                 # cn() 헬퍼, hasEnvVars 체크
└── supabase/
    ├── client.ts             # 브라우저 클라이언트
    ├── server.ts              # 서버(RSC/Server Action) 클라이언트
    ├── proxy.ts               # updateSession() — 루트 proxy.ts에서 사용
    └── database.types.ts       # Supabase CLI로 생성되는 타입 (gitignore 대상, 직접 수정 금지)
```

새 유틸리티는 실제로 여러 곳에서 재사용될 때만 `lib/`에 파일을 추가하세요. `env.ts`, `types/`, `hooks/`, `schemas/`, `api/` 같은 하위 구조는 현재 존재하지 않으며, 필요해지기 전까지 미리 만들지 마세요.

## 🏷️ 파일 네이밍 컨벤션

### 파일명 규칙

```bash
# ✅ 올바른 파일명
user-profile.tsx        # kebab-case (권장)
UserProfile.tsx         # PascalCase (컴포넌트)
userProfile.tsx         # camelCase (허용)

# ❌ 잘못된 파일명
user_profile.tsx        # snake_case (금지)
userprofile.tsx         # 소문자만 (금지)
```

### 컴포넌트 네이밍

```typescript
// ✅ 올바른 컴포넌트 네이밍
export function UserProfile() {} // PascalCase
export function LoginForm() {} // PascalCase
export function APIEndpoint() {} // 약어도 PascalCase

// ❌ 잘못된 컴포넌트 네이밍
export function userProfile() {} // camelCase (금지)
export function login_form() {} // snake_case (금지)
```

### 폴더 네이밍

```bash
# ✅ 올바른 폴더명
components/             # 소문자
user-settings/          # kebab-case
api-routes/            # kebab-case

# ❌ 잘못된 폴더명
Components/            # PascalCase (금지)
user_settings/         # snake_case (금지)
```

## 🔗 경로 별칭 (Path Aliases)

`components.json`에 정의된 경로 별칭:

```typescript
// ✅ 경로 별칭 사용 (권장)
import { Button } from '@/components/ui/button'
import { cn } from '@/lib/utils'
import { LoginForm } from '@/components/login-form'

// ❌ 상대 경로 사용 (금지)
import { Button } from '../../../components/ui/button'
import { cn } from '../../lib/utils'
```

**📍 정의된 별칭:**

`tsconfig.json`에는 `"@/*": ["./*"]` 하나의 와일드카드 별칭만 있습니다. `@/components`, `@/lib`, `@/components/ui` 등은 전부 이 규칙으로 해석됩니다. `components.json`의 `aliases`(components/utils/ui/lib/hooks)는 `shadcn` CLI가 컴포넌트를 생성할 때 파일을 어디에 둘지 결정하는 용도일 뿐, 별도의 tsconfig 경로가 아닙니다. `@/hooks`는 아직 대응하는 폴더가 없습니다(훅이 필요해지면 `lib/hooks/` 또는 `hooks/`를 새로 만드세요).

## 📝 새 파일/폴더 추가 규칙

### 1. 새 UI 컴포넌트 추가

```bash
# shadcn/ui 컴포넌트 추가
npx shadcn@latest add [component-name]

# 커스텀 UI 컴포넌트 추가
components/ui/custom-component.tsx
```

### 2. 새 페이지 추가

```bash
# 정적 페이지
app/about/page.tsx

# 동적 페이지
app/users/[id]/page.tsx

# 그룹 라우트
app/(marketing)/about/page.tsx
```

### 3. 새 비즈니스 컴포넌트 추가

```bash
# 위치 결정 기준:
1. 특정 페이지에서만 사용 → 해당 페이지 폴더 내(예: app/instruments/instruments-data.tsx)
2. 여러 페이지에서 사용 → components/ 루트에 평면 배치
3. shadcn/ui 원자 컴포넌트 → components/ui/
```

### 4. 새 유틸리티 추가

```bash
# 공통 유틸리티
lib/utils.ts            # 기존 파일에 추가

# 특화된 유틸리티
lib/date-utils.ts       # 새 파일 생성
lib/api-utils.ts        # 새 파일 생성
```

## 🎯 코드 조직 베스트 프랙티스

### 1. 단일 책임 원칙

- 하나의 파일은 하나의 주요 기능만 담당
- 관련된 타입과 유틸리티는 같은 파일에 포함 가능

### 2. 의존성 순서

```typescript
// 1. 외부 라이브러리
import React from 'react'
import { NextPage } from 'next'

// 2. 내부 라이브러리 (@/ 경로)
import { Button } from '@/components/ui/button'
import { cn } from '@/lib/utils'

// 3. 상대 경로
import './component.css'
```

### 3. Export 규칙

```typescript
// ✅ Named export 사용 (권장)
export function LoginForm() {}

// ✅ Default export (페이지 컴포넌트)
export default function LoginPage() {}

// ❌ 혼재 사용 지양
export function LoginForm() {}
export default LoginForm // 같은 컴포넌트를 두 방식으로 export
```

### 4. 파일 크기 관리

- 단일 파일: 300줄 이하 권장
- 300줄 초과 시 분할 고려
- 관련 기능별로 분리

## 🚫 금지사항

### ❌ 피해야 할 구조

```bash
# 깊은 중첩 구조 (4단계 이상)
components/pages/auth/forms/login/LoginForm.tsx

# 의미 없는 폴더명
components/misc/
components/common/
components/shared/

# 혼재된 케이스
Components/userProfile/LoginForm.tsx
```

### ❌ 피해야 할 패턴

```typescript
// 거대한 파일
export function SuperMegaComponent() {
  // 500줄 이상의 코드
}

// 혼재된 import
import Button from '@/components/ui/button' // default
import { Card } from '@/components/ui/card' // named

// 깊은 상대 경로
import { utils } from '../../../../../lib/utils'
```

## ✅ 체크리스트

새 파일/폴더 추가 시 확인사항:

- [ ] 적절한 카테고리 폴더에 배치
- [ ] kebab-case 파일명 사용
- [ ] PascalCase 컴포넌트명 사용
- [ ] 경로 별칭 사용
- [ ] 단일 책임 원칙 준수
- [ ] 적절한 export 방식 선택
- [ ] 의존성 import 순서 준수
- [ ] 파일 크기 300줄 이하 유지

이 가이드를 따라 일관성 있고 유지보수하기 쉬운 프로젝트 구조를 만들어보세요!
