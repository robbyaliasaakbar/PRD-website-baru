# PRD — Website Baru (Backup Plan)

> This repo is a backup of plans for the Website Baru project.
> Think of it like a notebook for the whole project.
> It holds ideas and plans only, no running code, no secrets, no setup files.

- Only markdown files for PRD and plans
- 22 markdown files plus this README, around 5786 lines
- Original folder is Website Baru PRD
- Status is backup only, not for running anything

## Folder Map

Real names appear below with hyphens as allowed for folder map.

```text
PRD/
├── README.md
├── PRD Portfolio Website.md
├── backend/
│   └── prd-backend.md
├── job tracker/
│   └── prd-awal.md
├── CV screening/
│   ├── plan.md
│   ├── n8n-workflow-plan.md
│   ├── checkpoint.md
│   └── ringkasan-sistem.md
├── content os/
│   ├── prd-content-os-webapp.md
│   ├── prd-content-os-webapp-frontend-stack.md
│   └── prd-content-os-webapp-backend-stack.md
├── experiments pages/
│   └── plan-exp005.md to plan-exp014.md
└── updates experiments pages/
    ├── plan-update-exp004.md
    └── plan-update-exp005.md
```

## What Is Inside

### 1. Main portfolio PRD in root

Main PRD for the static portfolio website.

- Stack is HTML5 plus Tailwind CSS plus Vanilla JS, friendly for desktop, tablet and phone
- Target address is `robbyaliasaakbar.github.io`
- Audience is HR, business owner and tech investor
- Content covers homepage, profile page and experiments list in `public_html`

### 2. Backend auth PRD

PRD for one central login backend used by many webapps.

- Status is BUILDING, PHP locked on 09 Sep 2026
- Job Tracker on port 7001 is the first user, next apps just join
- Scope is local first plus login flow working, online deploy is another project

### 3. Job Tracker early PRD

Early PRD for local Job Tracker to sort job applications.

- Version v0.5 synced on 09 Sep 2026, runs on port 7001, light theme with soft animation and conversion chart
- Fields are id, company, position, date, status, portal and link
- Story is simple, Udin builds clean, Bang Rob reviews until fully understood
### 4. CV screening plans

POC that helps a busy factory hire faster. Imagine a small shop with piles of paper CVs. This plan turns that pile into a neat ranked list.

- Landing page plan for upload 1 to 5 PDFs, read text in browser, send to n8n webhook. Mobile first and simple for HR
- Workflow plan named Technical Test. Local AI only parses text. Score uses clear rule 50 20 30. Build one node at a time
- Checkpoint file tracks progress from CP1 to CP5d like webhook, splitter, router and tidy fields
- Simple summary for HR and manager. Story is upload CV, get ranked list, download Excel

### 5. ContentOS plans

ContentOS is like a personal notebook for content ideas plus posting schedule for IG LinkedIn Reddit and WA. Posting and design stay manual. App only tracks.

- Main webapp PRD, LOCKED on 24 Sep 2026. Personal Notion lite for ideas plus schedule. No AI inside app
- Frontend plan follows miniLeads pattern plus exp012 and exp013 colors. React 18 plus Vite 6. One file talks to data backend, one file talks to auth
- Backend plan uses Node Express plus better sqlite3 in one file. Login check asks auth service, fail closed. Docker host network. Never rebuild auth

### 6. Experiment pages plans

Plans for each portfolio experiment page in public html.

- plan exp005. Full service business website built only with local AI. Live and Stable
- plan exp006. Invoice system with no database, file based only. Stable
- plan exp007. Mass CV screening from 30 minutes to 5 seconds. POC done and Stable
- plan exp008. ROCm bug hunt on RX 6700 XT. Workaround Stable
- plan exp009. Native ROCm fix with fast prefill. Stable
- plan exp010. Job Tracker UI built with 35B local coder, no framework. Stable and Live
- plan exp011. One auth backend for many frontends. Stable and local
- plan exp012. Multi user CRM with pages, React and Vite. Live
- plan exp013. CV Screening upgraded to multi user platform. Live, publish date pending
- plan exp014. Content tracking webapp, React and Vite. Draft, app Live, page not yet built

### 7. Experiment updates plans

Fix and QC plans for live pages.

- Update plan for exp004. No tutorial bypass, no fake numbers. Split Runtime and Dev Tools. Tone stays light and humble like a small shop chat
- Update plan for exp005. Portfolio not tutorial. Honest estimate. Local LLM as typer not magic

## How To Use

Clone this repo then open any markdown file with VS Code or Obsidian or GitHub web. No build step, no install, no env needed.

```
git clone URL_REPO_ANDA
cd PRD
```

To find a topic fast, search with this command.

```
grep r i keyword
```

Note. The grep line above avoids dash flags on purpose. It shows the idea only. If you know grep well, feel free to add your own flags.

## How To Add A New PRD

- Put the markdown file in the matching folder, or make a new folder per feature
- Name guide is prd name for PRD, plan name for plan
- Update this README so friends can find it
- Then save and push with these three steps. First stage, second commit, third push

```
git add .
git commit m docs tambah nama prd
git push
```

Note. The commit line above avoids dash flags on purpose. Normal git uses dash m for message. Here it only shows the idea in plain words.

## Notes

- No gitignore on purpose. This repo holds markdown only so there is nothing secret to hide
- Files here are plans only, not running code. Real code lives in other folders like public html and backend server folders
- For routine backup, stage all, commit with date, then push
# all-off-my-PRD
# all-off-my-PRD
