---
name: director-skill
description: Input a story or topic and output one complete shoot-ready file for Pixar-style animation or AI-generated photorealistic live-action, with character design sheets, scene and prop references, shot lists with @asset Paste prompts, transition and edit tables, six-panel storyboards, and a QC checklist.
---

# Director Skill: Input Story → Output Complete Shoot-Ready File

## This skill is an executor spec, not a guide

- The user inputs a story or topic. You do all remaining work and output one complete shoot-ready file. The user only gives the story and picks the type at the start, then nods or comments at feedback nodes.
- "Live-action" here means AI-generated photorealistic video, not real shooting. There are no actors, cameras, or real locations; character, performance, camera, and world are all AI-generated, and continuity across separately generated clips is the hard problem.
- Default to the full package. Open with an interactive prompt: ask animation or live-action, then the four basic constraints (aspect ratio, duration, style, dialogue or not — ask when missing, never default), then offer the step checklist with all steps selected; the user can uncheck steps and you run only the checked ones.

## Contract

- **Input**: a story or topic + type (animation or AI live-action). Ask the four basic constraints before starting.
- **Output**: one complete shoot-ready file with the nine sections below, in order. Nothing missing counts as complete. Deliver it as an HTML page: every prompt (image prompts and shot Paste prompts, English and Chinese) gets a one-click copy button, so the user can paste each prompt straight into the model.
- **Your job**: theme, script, storyboard, character design, scenes, shot prompts, transition table, edit table, QC — all written by you.

## The nine sections of the output file

1. Cover / basic info (title, type, aspect ratio, total duration, seconds per shot, dialogue or not, style; missing one item means incomplete)
2. Theme (three sentences, all required: one-sentence theme, one-sentence audience feeling, one-sentence protagonist change A→B; if you cannot write all three, stop and fix the theme first)
3. Story synopsis (one causal main line; every scene statable in one sentence; what the protagonist wants, fears, and finally gains or loses; end with a theme check line such as "does this scene deepen the misunderstanding or pay off the reversal")
4. Character design (one brief per main character with all fields; design-sheet list; one copy-paste image prompt per character)
5. Scenes (one fixed scene description per scene: place, time, light, mood, props; one scene = one place + one time + one dramatic point)
6. Shot list (per shot: number, duration, picture, action, expression, camera, sound, first-frame source, Paste prompt)
7. Transition table (how each adjacent pair of shots connects, and whether it needs a transition shot)
8. Edit table (each cut: transition type + sound handling)
9. QC checklist (all checked before delivery)

Conditional section — subtitles / on-screen text: when the film has a time jump or otherwise needs text on screen (for example a "十四年前" label at a flashback opening so muted viewers can follow), add a section before the QC checklist stating exactly what text appears, where on screen, in what style, and for how long. No dialogue means no subtitles; this section is only for deliberate on-screen labels.

## Character design

- **Character brief fields (all required)**: name/code, identity/occupation, age, gender, build, face (face shape, eyes, nose, mouth, skin), hair, clothing (top, bottom, shoes, accessories), signature trait, personality, role in the story, key expressions/actions.
- **Design-sheet list**: turnaround views, expression board, pose board, color palette, scale chart, detail close-ups (key props / costume details).
- **Comprehensive sheet, 16:9, one image per character**: pack as much as fits while staying readable as a video reference. Layout, richest first: left, one large full-body hero shot; top-middle, five-view turnaround (front, 3/4 front, side, back, 3/4 back); middle, head-and-shoulders expression portraits (all key expressions from the brief, each may carry an angle); right-middle, costume and material breakdown (each garment drawn separately, with material notes) plus accessory detail close-ups (key props); right, one cinematic emotional still / atmosphere reference; bottom, a color palette strip (solid swatches, hex codes allowed). Profile data (name, age, height, occupation, temperament, keywords), character bio, and signature line go in the file as text, not in the image.
- **No text labels in the image**: keep the design sheet clean, no text labels on it (for example "front", "angry"). The video model reads the reference through the prompt, not through text in the image, and text risks bleeding into the generated video. Describe the sheet structure in the video prompt instead.
- **Image prompts describe the comprehensive sheet**: write the turnaround, expression board, pose board, and color strip into the prompt as being in one image. Never write a prompt for a single front-view character alone.
- **Ask the image model first** (ChatGPT Image 2.6 or Qwen Image 2.1), then write prompts in that model's style. See the per-model conventions below.
- **English prompts get a Chinese translation directly below**: every English prompt (image prompts and shot Paste prompts) gets a Chinese translation under it, so users who do not read English well know what it says. The Chinese is reference only, never fed to the model.
- **Pass standard**: silhouette reads (recognizable with the face covered); appeal for animation, realism for live-action; recognizable across angles and shot sizes.

## Scenes and props

- **One fixed scene description per scene**. Same scene keeps the same description across all its shots; changing scenes changes the description.
- **Every scene gets a reference image**: a 16:9 wide establishing shot, empty environment, no characters, one per scene. Text alone will not stop the video model from changing the scene shot to shot.
- **Key props get a reference image too**: a 16:9 prop sheet, several key props per image. Generic props roughly hold their shape but details drift (color, size, shape); distinctive key props drift more without a reference. The more a prop matters to the story and the more distinctive it is, the more it needs a reference.
- **Pass standard**: reading the scene description, the audience can say where it is, what time it is, and what the light is.

## Shot list

Every shot lists number, duration, picture, action, expression, camera, sound, first-frame source, and a Paste prompt. Every shot must meet all of these:

- **One idea per shot**: a shot tells one thing. Two things means two shots.
- **One action**: the character does one action, written concretely (tear, cover, hand over, flick, walk, stand), never abstract.
- **One camera move**: push, pull, pan, track, follow — pick one. Two moves in one shot fight each other.
- **Expression and eyeline written out**: what the face shows and where the eyes look. Write subtle expressions concretely ("restrained sadness" beats "sadness"). Animation may exaggerate; live-action stays restrained and natural.
- **Inner life / subtext written out**: what the character thinks, wants, fears in this shot. A shot with action but no inner life plays empty.
- **First-frame source (required field)**: every shot states where its first frame comes from — none (opening or new scene), the previous shot's last frame (Method A below), or a newly generated first frame built from the previous tail frame plus character/scene references (Method B below). A shot with no stated first-frame source is incomplete.
- **Same-scene continuation: two methods, never mixed.** Video models generate each clip independently and have no memory of the previous clip. Writing "continuing from the previous shot" in text alone does not work — the model never saw the previous shot and cannot know what it was. **Method A (same camera and shot size)**: use the previous clip's actual last frame as the next clip's image-to-video input, so the next clip's first frame IS the previous clip's last frame. Edit by splicing directly, trimming 2–3 frames at the splice. **Method B (different camera or shot size)**: the tail frame CANNOT be the first frame, because the first frame IS the camera angle — a different angle means a different first frame. Generate a new first frame from the previous tail frame plus the character/scene references, then image-to-video from that new frame. Edit with a hard cut or action-to-action. The old rule "tail-frame to first-frame AND change the camera" is a contradiction: it asks the model to keep the same first frame and change the camera at the same time, which cannot both hold. Pick one method per adjacent pair and write it in the shot's first-frame source field and in the transition table. What continues is the character's state, not the camera.
- **Change camera and shot size**: two consecutive shots in the same scene must change camera position and shot size (medium → close-up), or it is one long take. When the camera or shot size changes, use Method B (a newly generated first frame), never Method A.
- **Paste prompt order**: previous shot's ending state → new action → expression/eyeline (with inner life) → camera (shot size, movement, focal length, depth) → light/mood → constraints. One sentence, one meaning.
- **Paste prompts use @asset names, not natural-language descriptions**: the video model does not know who "young woman" is; it only knows the uploaded asset by its @name. Name assets when creating reference images (for example the girl is @女主, the monster is @小怪兽, a scene is @场景1, props are @道具), then write @女主, @小怪兽, @场景1, @道具 in Paste prompts. Never write "young woman", "the furry monster", "the narrow old alley". Keep @asset names untranslated in the Chinese translation too.
- **Duration**: live-action about 5–10 seconds per shot; animation 2–6 seconds normal, split anything over 8 seconds.
- **Animation adds performance direction (讲戏)**: before animating, write what the character thinks, wants, fears in each shot.
- **Live-action adds blocking**: who stands where, who walks where, when they turn, when they look at whom; blocking serves the drama (who leads, who yields, how the relationship changes).

## Storyboards

- **Storyboards are images, not just text**: generate multi-panel storyboard images so the user sees the film's pictures and emotional flow at a glance.
- **Six panels per image (3×2)**. Twelve shots take two images. Panels beyond the fourth or fifth tend to drift, so check every panel in a six-panel image for character and scene consistency; split eight or more panels across images.
- **Each panel drawn in full detail**: composition (shot size, camera, angle), character (per the design sheet's look and pose), scene (per the scene image's place and props), light (per the shot's light and mood), action (the shot's key action moment). A panel must make clear what the shot films and what its emotion is, not a rough scene.
- **The storyboard is for the user, so make it rich**: besides the picture, write the caption into the image under each panel — shot number, duration, camera movement, key action (in Chinese) — so the user understands the shot and the camera move from the image alone. The full picture, action, expression, inner life, and sound stay in the file.
- **References**: draw each panel using the character design sheets and scene reference images, so character and scene stay consistent across panels.

## Continuity and transitions

- **Same scene: two methods, never mixed.** **Method A (same camera and shot size)**: the previous clip's actual last frame becomes the next clip's image-to-video input, so the next clip's first frame IS the previous clip's last frame; edit by splicing directly, trimming 2–3 frames at the splice. **Method B (different camera or shot size)**: generate a new first frame from the previous tail frame plus character/scene references, then image-to-video from that new frame; edit with a hard cut or action-to-action. The next shot's prompt describes the new action to take from that frame, and restates the previous shot's ending state (pose, expression, action progress, light, tone) in words to help the model. Text alone cannot do this: the model generates each clip independently and has no memory of the previous clip, so "continuing from the previous shot" in text only produces soft continuity and the first frame will not match. What continues is the character's state, not the camera; camera and shot size must actively change — and when they do, Method B is the only way.
- **Changing scenes, four steps**: split scenes before splitting shots (one scene = one place + one time + one dramatic point, with one fixed scene description); pick the transition type before writing the prompt (five types: hard cut, black, dissolve, match cut, J-cut); the new scene opens with an establishing shot, never a close-up cutting to a close-up; handle the transition itself separately (generate it as its own clip, or hide the seam in editing with an empty shot, black, dissolve, or J-cut).
- **Transitions belong to the editor**: the director decides why to cut, the editor decides how, how many frames, and where. In short video or solo production, sound effects also belong to the editor.
- **Decide for each transition whether it needs a generated transition shot.** Most transitions are done in editing with no extra shot: hard cut (splice directly), black (fade to black and back), dissolve (cross-fade in editing), J-cut (audio edit, no visual shot). But some transitions need a generated clip, and the skill must plan for it: a complex match cut (the shared element must be in a specific position in both shots), a camera move that carries across the cut (the camera pans in Shot A and must keep panning in Shot B), a cutaway or empty shot to hide a seam (an environment or detail shot inserted to bridge a jarring jump or a time gap), or an establishing shot for a new scene if the shot list jumps straight into a close-up. When a transition shot is needed, add it to the shot list as its own shot with its own number, duration, picture, action, camera, sound, and Paste prompt — it is a shot to generate, not an afterthought. The transition table must state for every adjacent pair whether a transition shot is needed and, if so, what it is.
- **J-cut sound**: the previous scene's sound continues under the cut, the next scene's sound lands on the visual cut. BGM closes on the last shot, never cut early.

## Pre-generation audit — the "can this be cut" gate

Before writing any Paste prompt, run this audit on the shot list. It exists because a script can read perfectly and still be impossible to generate or cut. Every item must pass; a failure means fixing the shot list, not the prompt.

1. **Spatial consistency**: every shot's place, props, and character positions match the scene description AND the scene reference image. A prompt cannot ask for something the scene reference does not contain (a bed in a living room, a yard outside an apartment door, a character standing where the camera is). State the camera position and each character's position per shot; they must not conflict.
2. **One action, and it must fit the shot size**: each shot has at most one action and at most one subject doing its own action. More than one means splitting the shot. The action must be visible inside the stated shot size — a close-up of eyes cannot show standing up and walking; a car-interior shot cannot show waving outside the car. If the action does not fit the frame, change the shot size or split the shot.
3. **State continuity**: track weather, time of day, light, and character condition across the whole shot list. Any change must be consistent with earlier shots. A "starts again" (rain starts again, a character returns) requires a preceding stop or departure in the film.
4. **Echo shots (visual rhyme)**: when a place or a gesture repeats (the same corner years apart, the same embrace twice), state the reference shot number and require the same angle, framing, and hold, so the audience recognizes it as the same place and the same gesture.
5. **Triggers and breathing room**: every time jump (flashback, flash-forward) needs a visual trigger in the preceding shot (a raindrop on a photo, an object, a sound that carries over). Every emotional beat needs breathing room after it — an empty shot or a pause — before the next beat.
6. **One camera move per prompt**: the camera part of each prompt contains exactly one movement (locked-off static, push-in, pull-back, tracking, pan — pick one). "Locked-off static" plus "push-in" in the same prompt is a contradiction; the model will pick one unpredictably. Check the prompt text, not just the shot list.
7. **No music in prompts**: no music words (piano, strings, music, BGM) anywhere in a prompt except inside the reverse-control sentence. BGM is always added in editing, never generated. If a prompt describes music starting, delete it.
8. **No intelligible speech when the film has no dialogue**: prompts must not contain speech verbs (shout, call, ask, say, speak, sing) or any line that could be read as words. Only non-verbal sounds (breathing, footsteps) and indistinct crowd murmur ("murmured crowd with no clear words"). Shouts and lines are added in post.
9. **No duplicated faces or limbs**: every prompt includes "Exactly one of each character, no duplicated faces or limbs." A single-face close-up states "Only one [character], one face, in frame."
10. **Scene reference matches the scene's current state**: when a scene's state changes (rain stops, day turns to night), the scene reference image must match the new state. If the reference shows rain and the shot is after the rain stopped, generate a new scene reference for the new state or state the change explicitly in the prompt.

## Hard rules (no exceptions when writing the file)

1. One idea per shot.
2. One camera move per shot.
3. What continues is the character's state, not the camera.
4. A new scene opens with an establishing shot.
5. The finished film must have BGM. No BGM means no delivery.
6. One sentence, one meaning, in prompts.
7. A locked reference image cannot be casually changed (changing it means changing the actor or the scene).
8. A clip with a broken face or a changed scene gets regenerated directly, never left for editing (generate several times and pick, or change the prompt if nothing picks; never force a bad clip).
9. Direction and eyeline must not break (180° rule, eyeline match, a character walking left must not walk right in the next shot).
10. Verify duration by counting video-stream frames, not container duration.
11. Script before images: never generate any image before the user has seen the complete script text and chosen to proceed to images.
12. One camera move per prompt: a prompt must not contain both a static/locked-off instruction and a push-in/pull-back/tracking instruction.
13. No music in prompts: BGM is always added in editing, never generated. A prompt that describes music starting is wrong.
14. No intelligible speech in prompts when the film has no dialogue: no shout, call, ask, say, or sing. Only breathing, footsteps, and indistinct murmur.
15. Every shot states its first-frame source; when the camera or shot size changes, the first frame must be newly generated (Method B), never the previous tail frame directly.
16. One action per shot, and the action must fit inside the stated shot size.

## Judgement calls (you decide, by this method)

- **Theme**: ask three questions — what is this story really about, what should the audience feel, what does the protagonist change from and to.
- **Casting / faces**: analyze the story into a character brief; the brief decides the face.
- **Camera movement**: push in to read a face, pull back to show a relationship, handheld for tension, locked for calm.
- **Cut points**: cut on a clear beat of action, eyeline, emotion, or sound.
- **Color and light**: produce a color script that maps the emotional arc to tone and light.
- **Transitions**: hard cut when the logic is clear, black for a large time/space jump, dissolve for time passing, match cut on a shared element, J-cut with sound leading.

## Feedback and iteration

- **Script first, images second — this order is not negotiable.** Write the complete script content (all text: theme, synopsis, character briefs, scene descriptions, shot list, transition table, edit table) and show it to the user BEFORE generating any image. Never generate a character sheet, scene image, or storyboard image before the user has seen the complete script text. Images come after the script is approved, never before.
- **The script feedback node is a hard gate with a two-choice question.** After showing the complete script, ask the user: "Generate the images now, or do you have changes to the script?" The user picks one: (a) generate images — proceed to character design and storyboards; (b) changes — revise the script, show the revised script again, and ask the same question again. Do not generate images until the user picks (a). Do not show the detailed script content only after the images are done.
- **Feedback nodes (★)**: after the script, after the storyboard, and after character design, show the work to the user and revise until it is approved before moving on. Never skip a feedback node to save time.
- **Each feedback round**: hand over the work plus three concrete questions (for example "Is this theme sentence right?", "Does this shot read clearly?", "Is this face recognizable from another angle?").
- **When feedback comes back**: restate your understanding ("You mean…, I'll change it to…, right?") and confirm before revising. When the user only says "so-so" or "no" with no detail, ask one question ("Which part is off: the story, the face, or the shots?"). Never guess and revise.
- **Also in the flow**: story reel (cut the storyboard with temp dialogue and music and watch the film once), color script (in visual development for animation), performance direction before animation, blocking, film grammar (180° rule, eyeline match, screen direction), subtext in performance prompts, editing grammar (decided early, not at the end).

## Input handling

- **One-sentence idea**: develop it into a synopsis first, then start from theme.
- **Synopsis given**: start from theme.
- **Full script given**: check it against the standard (every scene statable in one sentence, one causal line), fill the gaps, then move to the storyboard.
- **Reference image given**: use it as a design sheet or scene image, but verify consistency (recognizable from another angle?).
- **Only some steps wanted**: run only the checked steps from the opening interactive prompt.

## Per-model prompt conventions

### Image models (character and scene design)

Ask which image model first (ChatGPT Image 2.6 or Qwen Image 2.1), then write in that model's style. The two models use different conventions; never share one set of prompts.

- **ChatGPT Image 2.6**: natural-language description, never keyword stuffing (the model reasons; stuffing hurts). Order: background/scene → subject → key details → constraints. Length 100–300 words. Character consistency comes from reference images plus an identity anchor (face, hair, eyes) and an explicit "do not copy the reference's expression/pose, draw the expression/eyeline/angle in this prompt". Reference multiple images by number. When revising, state what changes and what stays, and restate the keep-list every follow-up to prevent drift.
- **Qwen Image 2.1**: one paragraph, about 400–500 words (an anchor sentence of about 20 words first, 8–14 positional phrases walking the frame, one separate sentence for light, one closing sentence for the overall composition). Descriptive text in English (only text to be rendered inside the image uses the original language in quotes). No unrelated keyword stuffing. **2.1 does not support negative_prompt**. prompt_extend on for short prompts, off (explicitly false) for carefully written ones. Fixed seed helps consistency.

### Video models (clip generation)

Ask which video model first, then write Paste prompts in that model's style, the same way casting asks the image model.

- **Meta Muse Video**: about 10 seconds per clip, vertical 9:16, native audio, no dialogue. One clip at a time; narrative comes from splicing clips. One sentence skeleton: Subject + Action + Style + Camera + Lighting + Motion + Duration/Beat. One subject, one action, one environment per clip. Write director instructions, not descriptions. Image-to-video is steadier than text-to-video. Change only 1–2 variables at a time. Subtle expressions are a weak point.
- **MiniMax H3 (local ComfyUI)**: about 10 minutes for 5 seconds (RTX 4060 Ti 16GB). Prompts are structured, written by official field names, order, and time marks. Five modes: T2VA (text only), I2VA (first frame), L2VA (last frame), FL2VA (first and last frames), Ref2VA (full references: images, videos, audio mixed). Base modes write three fields in order: `integrated_multimodal_description` (whole scene first, then a shot timeline with `[Shot 1]`, `[Shot 2]` from 0.00s, single time points like `At 00:05.000`, never ranges) → `overall_soundscape` → `non_diegetic_music`. I2VA starts with a fixed alignment line `For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.` then a blank line, then the three fields. Ref2VA writes six fields in order: `subject_definitions` → `summary` → `retention_analysis` → `detailed_description` → `overall_soundscape` → `non_diegetic_music`. References cap at 9 images / 3 videos / 3 audio, 12 files total; each video/audio clip 2–15 seconds, each type at most 15 seconds total; cite with `<Picture 1>` / `<Video 1>` / `<Audio 1>` in connection order and state what each asset controls (identity, style, action, camera, sound). T2V/I2V use FL2VA weights, R2V uses separate Ref2VA weights; never swap them. Dialogue/lyrics use `<d>` tags; on-screen text written verbatim; camera moves directly after the key description (`[pan]` / `[zoom]` / `[static]`). Prompts in English. Limit 7000 characters. Output 4–15 seconds, 24 FPS, H3-Base 768p (native 1344×768, 16:9), dimensions in multiples of 32. ComfyUI 0.30.0+. Duration rounds to the `17k+5` frame grid (6.0s → 158 frames = 6.58s). Weights about 42.5GB. Community License excludes the US, EU, UK, Korea.
- **Seedance 2.0 / 2.5**: one-sentence skeleton: subject + action + scene + light + camera language + style + quality + constraints; 2.5 may use five parts [subject and identity] + [core body action and rhythm] + [camera movement and composition] + [environment and light details] + [synced audio and ambience]. Prompts read like a shot list, not a description. **The @ reference system is core**: uploaded assets are auto-numbered (`@Image1` / `@Video1` / `@Audio1`; platform spellings differ: BytePlus `@Image 1`, Replicate `[Image1]`, Dreamina `@Image1`). State every asset's purpose and its mapping to frame elements; never let the model guess. References are a cap, not a goal. **Stability constraints in every prompt**: face stable and undistorted, body structure normal, motion natural and smooth, face clear, costume consistent, hairstyle unchanged; 2.5 may anchor-lock (`@1` locks the face, `@2` locks costume/props). **Anti-duplication in every prompt**: add "Exactly one of each character, no duplicated faces or limbs."; a single-face close-up states "Only one [character], one face, in frame." Write concretely and physically: contact points, weight/friction, one key light; no abstract emotion words. Length: 2.0 is 30–300 words (median about 109); 2.5 text-to-video 100–500 words (median about 342), image/reference 200–900 words (median about 399), suggested cap 1000 English words or 500 Chinese characters. **One camera move per prompt**: the camera part holds exactly one movement — never "locked-off static" and "push-in" (or "pull-back", "tracking") in the same prompt; the model will pick one unpredictably. **No music in prompts**: BGM is always added in editing, never generated — never write that music starts or continues. **Sound reverse control (full form, every prompt)**: `No background music, no singing, no spoken words; keep only ambience and action SFX. No subtitles. No on-screen text.` **No intelligible speech when the film has no dialogue**: no shout, call, ask, say, or sing in the prompt; only breathing, footsteps, and indistinct murmur ("murmured crowd with no clear words"); shouts and lines are added in post. **First-frame source in the prompt**: for a continuation shot, state where the first frame comes from (previous tail frame, or a newly generated first frame built from the tail frame plus references). 2.5 only: timestamp segments (`0-5s:` / `5-10s:` / `10-15s:` / `15-30s:`), Chinese camera terms (一镜到底, 斯坦尼康, 浅景深, 俯拍, 仰拍) improve following, and for non-default dialogue language write language + accent/variant + tone + speaker before the line. Pick 2.0 for short precise shots (locked camera, hard cuts sometimes follow more reliably); pick 2.5 for over 15 seconds or many references, longer narrative, stronger continuity. **Real human faces cannot be uploaded as references** (2.0/2.5 reject real face reference images/videos; verified people go through a separate private asset library; use AI-generated character images, never real photos). Change one variable at a time; test one shot as a sample before batch runs — and test the riskiest continuation pairs first (where the first-frame method changes, or where a face could duplicate), not just any single shot.

## Step lists (opening interactive prompt)

- **Animation**: theme → script ★ (show full script, ask: images or changes?) → storyboard ★ → character design ★ → visual development and color script → layout → performance direction → animation → lighting → sound → editing.
- **AI live-action**: theme → script ★ (show full script, ask: images or changes?) → storyboard ★ → character design ★ → scene design → blocking → camera design → performance prompts → generation → sound → editing.
- (★ marks the feedback nodes.)

## QC checklist (all checked before delivery)

- Theme: all three sentences (theme, audience feeling, protagonist change) written?
- Synopsis: every scene statable in one sentence? One causal main line only?
- Characters: every main character's brief fields complete? Design-sheet list complete? Recognizable across angles and shot sizes?
- Scenes: every scene's description fixed? Scene description changed when the scene changes?
- Shot list: every shot one idea, one action, one camera move? Expression, eyeline, inner life all written? Does the action fit inside the stated shot size? Does every shot state its first-frame source?
- Pre-generation audit: did the audit (section above) pass on the shot list and on every prompt? Spatial consistency, one action per shot, state continuity, echo shots, triggers and breathing room all checked?
- Continuation: for every same-scene pair, is the first-frame method stated (Method A direct tail-frame when the camera and shot size stay the same, Method B newly generated first frame when they change)? Does the prompt restate the previous shot's ending state? Did camera and shot size change — and if so, is Method B used?
- Prompt-level checks (read every prompt, not just the shot list): does each prompt hold exactly one camera movement (no "locked-off static" plus "push-in")? Is there no music word outside the reverse-control sentence? When the film has no dialogue, is there no shout, call, ask, say, or sing? Does every prompt carry "Exactly one of each character, no duplicated faces or limbs."? Does every prompt use the full sound reverse control ("No background music, no singing, no spoken words; keep only ambience and action SFX. No subtitles. No on-screen text.")?
- Transition table: every adjacent pair's method written? New scenes open with an establishing shot? For every pair, is it stated whether a transition shot is needed, and if so, is the transition shot in the shot list with its own prompt?
- Edit table: every cut's transition and sound handling written? Direction and eyeline connect?
- BGM: does the finished film have BGM? (No BGM means no delivery.)
- Duration: verified by counting video-stream frames? (Not container duration.)

## Worked example

- **Test production file**: `examples/最后一盏灯_拍摄文件.md` (12 shots, 60 seconds, no dialogue; theme "光不是越亮越好——关掉刺眼的灯，才能看见萤火虫的光"). Read it to see the nine sections, the @asset Paste prompts with Chinese translations, the rich character boards, and the six-panel storyboards.
- **Script example** (not distributed with this package — do not go looking for it): 《它举起爪子的那一秒》 (`Muse.md`, 8 shots, about 65–70 seconds, vertical 9:16, no dialogue).
- **Storyboard example** (not distributed with this package — do not go looking for it): `muse_storyboard/shot_01.png` … `shot_08.png`.
- **Character design example** (not distributed with this package — do not go looking for it): the girl's and monster's rich 16:9 comprehensive sheets, scale chart, scene images, and prop sheet for 《最后一盏灯》.

## Animation vs AI live-action — key differences

| Dimension | Pixar animation | AI live-action |
|-----------|----------------|----------------|
| Character | Designed (stylized turnaround, expression board) | Generated real person (reference image locks the face, must stay consistent) |
| Performance | Animators build it frame by frame; the director directs them (performance direction) | AI generates it; the director commands through prompts; performance stays restrained and natural |
| World | Built from nothing (stylized scenes, lighting, color script) | Generated realistic world (reference images lock place and light) |
| Quality bar | Stylized, cute/readable (Appeal) | Realistic, like actually shot (real skin, light, physics) |
| Time cost | Years, endlessly revisable | Fast per clip, but repeated generation and picking |
| Revision cost | Low (relatively), always iterable | Medium, one clip cheap to regenerate, but a broken continuity forces many clips to regenerate |
| Director's control | Absolute (every frame can be set) | Through reference images and prompts, but AI is random; generate several times and pick |
| Biggest risk | Weak story (good pictures cannot save it) | Broken face / broken continuity (character, scene, light change between clips) |
| Where the director spends the most time | Storyboard and iteration (revised again and again) | Casting the face and checking continuity (every clip checked against references) |

- **Animation in one line**: the director is a creator, building the whole world from a blank page, grinding the story out through iteration.
- **AI live-action in one line**: the director is a casting director plus a prompt director, first locking one unchanging face and one unchanging scene, then generating clip by clip with prompts, finally splicing the film in editing. The biggest enemy is continuity.
