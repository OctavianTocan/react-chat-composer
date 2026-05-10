# @octavian-tocan/react-chat-composer

Themable React composer for AI chat UIs — text input, file attachments,
model + reasoning picker, voice recording, and empty-state prompt suggestions.
Built on Tailwind v4.

> **Status: scaffold (v0.1.0)** — see [TODO](#status--roadmap). The package is
> being lifted out of pawrrtal in stages and is not yet feature-complete on
> npm; consume it from the
> [`feat/extract-react-chat-composer`](https://github.com/OctavianTocan/ai-nexus/tree/feat/extract-react-chat-composer)
> branch via the git submodule under `frontend/lib/react-chat-composer/`.

## Installation

```bash
pnpm add @octavian-tocan/react-chat-composer
# peer deps
pnpm add react react-dom clsx tailwind-merge lucide-react \
  @octavian-tocan/react-dropdown @radix-ui/react-tooltip
```

Requires **Tailwind CSS v4**.

## Quick Start

```tsx
import { ChatComposer, CHAT_MODELS_2026 } from '@octavian-tocan/react-chat-composer';

export function MyChat() {
  const [text, setText] = useState('');
  return (
    <ChatComposer
      defaultValue=""
      onChange={setText}
      onSubmit={({ text, attachments }) => sendMessage(text, attachments)}
      models={CHAT_MODELS_2026}
      selectedModelId={modelId}
      onSelectModel={setModelId}
      onTranscribeAudio={async (blob, mime) => {
        // Plug in your STT provider (Whisper, Deepgram, xAI, etc.)
        return await transcribe(blob, mime);
      }}
    />
  );
}
```

## Setup

In your global CSS (typically `app/globals.css`):

```css
@import "tailwindcss";

/* 1. Tell Tailwind to scan the package's compiled output for utility classes. */
@source "../node_modules/@octavian-tocan/react-chat-composer/dist";

/* 2. Bring in the chat-* token defaults (light + dark). */
@import "@octavian-tocan/react-chat-composer/styles/theme.css";

/* 3. Bring in the non-Tailwind keyframes + animation classes. */
@import "@octavian-tocan/react-chat-composer/styles/animations.css";

/* 4. (Optional) Override any chat-* token to theme the composer. */
:root {
  --color-chat-accent: #ff6b35;
}
```

## Public API

- `<ChatComposer />` — the flagship component
- `<ChatPromptSuggestions />` — empty-state suggested-prompt list
- `defineChatModel()` + `CHAT_MODELS_2026` — model-list helpers
- `/primitives` — `<ComposerActionSelector />`, `<ProviderLogo />`,
  `<VoiceMeter />`
- `/hooks` — `useVoiceRecording()`
- `/types` — all public types

See [docs/API.md](./docs/API.md) for the full surface (forthcoming).

## Status / roadmap

- [x] Package scaffold + tsup config + tsconfig + releaserc
- [x] Submodule integration with `OctavianTocan/ai-nexus`
- [ ] Vendor `prompt-input-*` from AI Elements (form, textarea, attachments,
      footer, submit)
- [ ] Minimal `Button` + `Tooltip` from shadcn
- [ ] `usePersistedState`, `useTooltipDropdown`, `cn` utility
- [ ] `useVoiceRecording` (recording lifecycle + `onTranscribeAudio` swap)
- [ ] Move ChatComposer / ChatComposerControls / ModelSelectorPopover code
- [ ] Author `styles/theme.css` (`@theme` block, `chat-*` namespaced tokens)
- [ ] Author `styles/animations.css` (composer-placeholder, waveform-scroll)
- [ ] Bundle 8 monochrome provider SVGs into `<ProviderLogo />`
- [ ] Storybook + visual regression
- [ ] First npm publish

## License

MIT — see [LICENSE](./LICENSE).
Vendored code from Vercel AI Elements and shadcn/ui — see [NOTICE.md](./NOTICE.md).
