# Skills

Claude skills by Josh Janse, packaged for the [`skills` CLI](https://skills.sh).

## Skills in this repo

| Skill | What it does |
| --- | --- |
| [`explainer`](skills/explainer/SKILL.md) | Makes short animated explainer videos in the style of Vox, starting from the story and script beats before any visuals. |

## Install

Install one skill:

```sh
npx skills add josh-janse/skills --skill explainer
```

Install every skill in the repo:

```sh
npx skills add josh-janse/skills --all
```

To test a local checkout, run this from the repo root:

```sh
npx skills add ./ --skill explainer
```

## Layout

Each skill lives in `skills/<name>/`, with its `SKILL.md` at the top of that folder. The folder name matches the `name:` field in the frontmatter. Files whose names start with `_` are not installed.

## Licence

There is no licence file yet. Josh will choose one before the repo is made public.
