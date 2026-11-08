# Director Skill

Input a story or topic, output a complete shoot-ready file for Pixar-style animation or AI-generated photorealistic live-action.

## What it does

This is an executor spec, not a guide. You give it a story and a type (animation or AI live-action), and the AI does all remaining work: theme, script, storyboard, character design, scenes, shot prompts, transition table, edit table, and QC — and hands you one complete file you can shoot from.

## Install

```bash
npx skills add https://github.com/bugjiang82/director-skill
```

Or clone the repo and copy the `director-skill/` folder into your skills directory.

## How it works

1. You give a story or topic and pick animation or AI live-action.
2. The AI asks for the four basic constraints (aspect ratio, duration, style, dialogue or not) and offers a step checklist (all selected by default; uncheck steps you don't want).
3. The AI writes the full shoot-ready file: 9 sections, in order — cover, theme (3 sentences), synopsis, character design, scenes, shot list, transition table, edit table, QC checklist.
4. At three feedback nodes (script, storyboard, character design), the AI shows its work and revises until you approve.

## What the skill enforces

- **One idea, one action, one camera move per shot.**
- **Rich character boards (16:9)**: hero shot + five-view turnaround + expression portraits + costume/material breakdown + accessory close-ups + cinematic emotional still + color palette strip. No text labels in the image; profile data and bio go in the file as text.
- **Scene and prop reference images** (16:9) so the video model locks the scene and key props.
- **Paste prompts use @asset names** (e.g. `@女主`, `@小怪兽`, `@场景1`), never natural-language descriptions of people or places. Every English prompt gets a Chinese translation below it.
- **Storyboards**: 6 panels per image (3×2), each panel with its caption in the image (shot number, duration, camera movement, key action).
- **Continuity**: tail-frame to first-frame is the method — the previous clip's actual last frame is the next clip's image-to-video input. Text-only "continuing from the previous shot" does not work; the model has no memory of the previous clip.
- **Transitions**: most need no generated transition shot (hard cut, black, dissolve, J-cut are done in editing). A generated transition shot is needed only for a complex match cut, a camera move carrying across the cut, a cutaway to hide a seam, or a missing establishing shot — then it is a numbered shot in the shot list.
- **Hard rules**: BGM required, duration verified by counting video-stream frames, reference images locked once set, broken faces/scenes regenerated (never forced through editing), 180° rule and eyeline match.

## Per-model prompt conventions

- **ChatGPT Image 2.6**: natural language, background → subject → details → constraints, 100–300 words, identity anchor + indexed reference images.
- **Qwen Image 2.1**: one ~400–500-word paragraph, anchor + 8–14 positional phrases + lighting + composition, English prose, no negative_prompt, prompt_extend off for careful prompts, fixed seed.
- **Meta Muse Video**: ~10s per clip, 9:16, native audio, no dialogue.
- **MiniMax H3** (local ComfyUI): structured prompts, five modes (T2VA/I2VA/L2VA/FL2VA/Ref2VA), 4–15s, ~42.5GB weights.
- **Seedance 2.0 / 2.5**: @ reference system, stability constraints, timestamp segments (2.5), Chinese camera terms (2.5).

## Files

- `SKILL.md` — the skill itself.
- `skill.yaml` — skill metadata.
- `standards.md` — the full standards document (Chinese), the source the skill was built from.
- `examples/最后一盏灯_拍摄文件.md` — a worked example: a 12-shot, 60-second Pixar-style short with @asset Paste prompts, rich character boards, and six-panel storyboards.

## License

MIT
