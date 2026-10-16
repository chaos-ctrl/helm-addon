# Helm

Your private life hub. Reached only through Tailscale (your own private network), never the open internet.

## Install (from the Windows PC)

1. In GitHub Desktop: File > Clone repository > `chaos-ctrl/helm` > Clone (first time only; afterwards just
   press **Fetch origin** then **Pull origin** to get updates).
2. In that folder, open `deploy\homeassistant` and double-click **deploy.bat**. It copies Helm into
   Home Assistant (takes a minute or two). It ends with "Done": press any key to close the window.
3. In Home Assistant: **Settings > Add-ons > Add-on Store**, the three dots at the top right > **Check for
   updates**, then reload the page. **Helm** appears under "Local add-ons": open it, press **Install** (5 to
   10 minutes the first time; the button turns into a spinner, then the page shows Start).
   For later updates: run deploy.bat again, then press **Rebuild** on the add-on page.

## First start

1. **Tailscale HTTPS on** (once for the whole tailnet; probably already on for Tend): open
   https://login.tailscale.com/admin/dns, scroll to **HTTPS Certificates**. If it says **Enable HTTPS**, press it.
2. **Auth key**: open https://login.tailscale.com/admin/settings/keys > **Generate auth key...**. Description
   "helm". Leave Reusable, Ephemeral and Tags off. Press **Generate key** and copy it (starts `tskey-auth-`).
3. In this add-on, open the **Configuration** tab, paste the key into **Tailscale auth key**, check the
   time zone, press **Save**. Go back to the **Info** tab and press **Start**.
4. Open the **Log** tab and wait about a minute (press the refresh arrow). It prints
   "Helm's private address: https://helm.….ts.net". Write that address down.
5. **No expiry**: open https://login.tailscale.com/admin/machines, find **helm**, press **⋯** >
   **Disable key expiry**. (Otherwise Tailscale disconnects Helm after 6 months.)
6. On a phone or computer with Tailscale switched on, open that address. Create your account: username,
   password (15 characters or more), then scan the QR code with your authenticator app (Google
   Authenticator, Aegis, 2FAS...). Save the 8 recovery codes somewhere safe (a password manager, or printed).

The key is only needed once; Helm remembers the connection afterwards (you can empty the field).

Why Tailscale only? Until the first account exists, whoever opens Helm first becomes its owner. Keeping it
off the home network means nobody else can get there first, and later nobody on the Wi-Fi can even try
to guess a password.

## Security alerts by email

Helm emails you straight away when something odd happens: a sign-in from a new device, a recovery code
used, several wrong passwords, your right password with wrong codes, a password change.

Best set-up: **a separate Gmail account that only sends** (for example a spare one you already have), and
alerts arrive in your usual inbox. Then the only Google password Helm ever holds is a 16-letter "app
password" of that spare account, which you can cancel at any moment; your main account is never touched.
(Google lets a program send mail only with an app password or a full Google Cloud set-up; the app
password is by far the simpler and is limited to mail.)

1. Open a private window (Chrome: Ctrl+Shift+N) and sign in to https://myaccount.google.com with the
   **sending** account. Making sure it's that account: its picture or initial at the top right.
2. **Security** (left menu) > **2-Step Verification**: switch it on if it isn't (Google sends a code to
   your phone). App passwords exist only when this is on.
3. Open https://myaccount.google.com/apppasswords (same window). App name: **Helm** > **Create**. Google
   shows a 16-letter password once, in four groups: copy it (spaces don't matter).
4. In this add-on's **Configuration**:
   - **alerts_to_this_address**: your usual address (where alerts arrive).
   - **alerts_from_this_gmail**: the sending account's address.
   - **alerts_from_gmail_app_password**: the 16 letters.
   **Save**, then **Restart** (Info tab).
5. In Helm: Settings > Account > Security alerts by email > **Send a test**. It should arrive within a
   minute (check Spam the first time and mark it "Not spam").

To stop Helm from sending email, delete the app password on that Google page; Helm then simply notes
"alert not sent" in Settings > Activity.

## AI (optional)

Some buttons use an AI ("Smaller first step", "Do it with me", "Why am I stuck?"). Helm works without one.
Out of the box: **MiMo V2.6 Pro** (Xiaomi, through OpenRouter, cheap) for everything, and **Claude Sonnet 5.5**
(Anthropic) for cards in the **Work** and **Life** spaces and for the board. All can be changed in Helm > Settings > AI.

Privacy tip: in OpenRouter's **Settings > Privacy**, you can require **zero data retention** (the AI host keeps no
copy of what Helm sends). Then only models with such a host work: MiMo, DeepSeek and Claude do; Qwen Flash doesn't.
The list: https://openrouter.ai/api/v1/endpoints/zdr. Helm > Settings > AI > **Add a model** searches it for you
(with prices) and adds a model in one tap.

1. **OpenRouter** (for MiMo): https://openrouter.ai, sign in (Google is fine) > **Credits** > add a small
   amount (5 dollars lasts a long time) > **Keys** (https://openrouter.ai/keys) > **Create key**,
   name "Helm", credit limit e.g. 5 > **Create**. Copy the key (starts `sk-or-`); it is shown once.
2. **Anthropic** (for Sonnet): https://console.anthropic.com, sign in > **Billing**: add a payment method
   and some credit, and under **Limits** set a monthly spend limit (e.g. 10 dollars) > **API keys**
   (https://console.anthropic.com/settings/keys) > **Create key**, name "Helm" > copy it (starts `sk-ant-`).
   This is separate from a claude.ai subscription: the API is paid per use.
3. In this add-on's **Configuration**: paste them into **OpenRouter key** and **Anthropic key**, **Save**,
   **Restart**. With only one of the two keys, Helm uses that one for everything.

Every AI request is listed in Settings > Activity (what for, which model; never the content).

**Your own AI service (optional, no lock-in):** any service that speaks the OpenAI format works too: OpenAI, Gemini
(`https://generativelanguage.googleapis.com/v1beta/openai`), or one running at home such as Ollama
(`http://homeassistant.local:11434/v1`). In the add-on Configuration fill **Your own AI service, address** (and the
**key**, which a service at home usually doesn't need), Save, Restart; then in Helm > Settings > AI choose
"Your own AI service" in "Add a model" and type the model's name (for example `llama3.1:8b`).

**Passkeys:** Settings > Account and security > Passkeys > "Add a passkey on this device" lets you sign in with your
fingerprint, face or screen lock. It needs Helm's secure address (the Tailscale one, `https://...ts.net`). Your password and
authenticator code always still work.

**Notes from Google Keep:** on your PC run `python tools/keep_to_cleanup.py <the Takeout's Keep folder> <the helm-import
folder>`; the notes then appear, one at a time, on the reminders clean-up page.

## Job agent

The **Jobs** cards (Work space) come from the job agent's own repository on GitHub: applications ready to
send, drafts waiting in Gmail, interviews, the agent's questions and its to-do list. Helm reads them with a
key that can only **read** that one repository.

1. On a computer, open https://github.com/settings/personal-access-tokens/new (sign in to GitHub if asked).
2. **Token name**: `Helm (read only)`. **Expiration**: 1 year (Helm shows a card when it stops working).
3. **Repository access**: **Only select repositories** > pick **job-search-agent**.
4. **Permissions** > **Repository permissions** > **Contents**: **Read-only**. Leave everything else as it is.
5. **Generate token**, then copy it (starts with `github_pat_`); GitHub shows it only once.
6. In this add-on's **Configuration**: paste it into **github_read_token**, check **job_agent_repo** says
   `chaos-ctrl/job-search-agent`, **Save**, then **Restart** (Info tab).
7. Open Helm's Home: the job cards appear within a minute (Helm checks for changes every 15 minutes).

**Telling the agent "applied" / "skip"** (replaces the agent's old phone panel). With this second key,
Done on an "Apply:" card marks the job applied, Let it go marks it skipped (the agent applies them within
the hour); Done on one of its questions accepts the proposed answer; Done on one of its to-dos ticks it.

1. Open https://github.com/settings/personal-access-tokens/new again.
2. **Token name**: `Helm (job agent, write)`. **Expiration**: 1 year.
3. **Repository access**: **Only select repositories** > **job-search-agent** (only this one).
4. **Permissions** > **Repository permissions** > **Contents**: **Read and write**. Nothing else.
5. **Generate token**, copy it, paste it into **job_agent_write_token** in this add-on's Configuration,
   **Save**, **Restart**.

It's a separate key on purpose: the read-only one can see more repositories but change nothing; this one can
only write to the job agent, where Helm only ever adds small answer files.

Later connectors reuse the same key: edit the token on GitHub and add their repository (below).

**Without the agent** (Job search > "Move my job search into Helm", or a new Helm), Helm keeps the job list
itself: company career sites, an optional alert mailbox, scoring, documents. Two pages more, reached from
the links at the bottom of Job search:

- **📡 Launch radar** (for jobs in pharma or biotech; off until switched on in **What I'm looking for >
  Launch radar**): once a week Helm reads the European Medicines Agency's public list of medicines and
  shows the companies that got a new medicine approved or recommended in the last 60 days (they usually
  hire their medical team around the launch, often before advertising), plus biotechs that raised money
  (from the news feeds you list). Watch / Approached / Not for me on each, and ✍️ Approach drafts a first
  message (Helm sends nothing). A Helm moved from the agent copies the agent's radar and salary benchmarks
  once by itself.
- **📈 Job market**: matching jobs per week, the trend (after 8 weeks of history), who is hiring and where
  the matches come from, all from Helm's own list.

## People

**People** is part of Helm: who to reach out to, birthdays and anniversaries, "ask how it went", and the **Everyone** page where you add and change people,
log a contact, log a group, import a WhatsApp chat or phone contacts. The data lives inside Helm (one Markdown file per person in Helm's own database), is in
Helm's nightly backup and in **Settings > Your data, in plain files** (`people/` in the zip). Nothing else is needed: no GitHub, no key.

- **Bring people in from elsewhere:** Everyone > **⋯ > Import people (zip)**: a zip with `people/<name>.md` files (and `settings.yml`). People you already have are kept.
- **Take them out again:** Everyone > ⋯ > **Download a backup (.zip)** gives the same kind of zip.
- **Optional, for people who keep their data in a GitHub repository:** put the repository name (`owner/name`) in the add-on option **my_people_repo**, switch **my_people_sync** on, a read key in
  **github_read_token** and, to save changes, a write key in **my_people_write_token** (Contents: read and write on that repository only). Helm then reads and
  writes that repository instead of its own copy. Leave **my_people_sync** off to keep everything in Helm (the default).

## AI-model report

Helm makes this report itself, with nothing to set up. Every week it compares OpenRouter's public list of AI models
that keep no data with last week's copy and with the models you use: new models, price changes, a model that lost
its no-data hosts, and the cheapest options. It shows the result as a card in **Life** and on the **🤖 AI models**
page. It compares prices and availability only; it cannot judge quality, and it says so.

Optional, for someone who runs a separate report repository: put its name (`owner/name`) in **ai_report_repo** and
give Helm's read-only key access to it. Helm then shows that repository's `data/panel.json` instead. Leave
**ai_report_repo** empty to use Helm's own report (the default).

## Health, Money and People (inside Helm)

Three apps that used to run on their own now live inside Helm, behind Helm's own sign-in:
- **🌱 Health** (was Tend): sleep, mood, workouts, habit tiles, insights, the gym coach and the private medication log. Page: Me > Health
  (`#/health/today`). Its server is part of Helm (address `/health/api/...`) and its data is in Helm's data folder (`tend/`). Connect Google Health under
  Health > More > Connections (the redirect address to register at Google is shown there).
- **🏦 Money details** (was the wealth tracker): net worth, advice, spending, loans. Page: Life > Money details (`/money/`). Its files are in Helm's data
  folder (`money/config` and `money/data`). To link another person to their own money, add `helm_users: [their Helm username]` to their entry under `owners` in `money/config/config.yaml`.
- **👥 People** (was My People): the Everyone page and its tools (WhatsApp import, phone contacts, data check, backup zip). The data lives in Helm
  (see "People" above).

Cards from Health and Money reach Helm through the share (`\\homeassistant.local\share\helm-feeds`, one file per app) and show under "From your apps". Health's
cards (evening check-in, gym day, take it easy, Monday review) land in the Me space and open Health.

## Still-alive alert (healthchecks.io)

Helm sends a tiny "still alive" signal every 5 minutes. If the signals stop (Helm stopped, the Home
Assistant machine is off, or the home internet is down), healthchecks.io emails you within about 10 minutes.

1. Open https://healthchecks.io and **Sign Up** (free) with the email where you want the alerts.
2. In the project it creates, press **Add Check**. Name: `Helm`. **Schedule**: Period **5 minutes**,
   Grace **10 minutes**. Save.
3. On the check's page, copy its **ping URL** (starts with `https://hc-ping.com/`).
4. In this add-on's **Configuration**: paste it into **Still-alive signal (healthchecks.io)**, **Save**, **Restart**.
5. Within 5 minutes the check turns green. Optional: under **Integrations**, add a phone channel too
   (ntfy, Telegram, Signal...), so the alert also reaches your phone.

## Updating from anywhere (Helm from GitHub)

Installed as a *local* add-on, Helm is updated from a computer on the home network (deploy.bat). Installed
from GitHub, Home Assistant shows **Update** on Helm by itself whenever a new version is out, wherever you
are; nothing to copy. The app comes ready-made (no 10-minute build on the Home Assistant machine).

Moving from the local add-on to the GitHub one, once (about 10 minutes, at home):

1. Settings > Add-ons > Add-on Store > ⋮ > **Repositories** > add `https://github.com/chaos-ctrl/helm-addon` > **Add**.
2. In the share, open the folder `\\homeassistant.local\share\helm-move` and create an empty file named `EXPORT` (no extension).
3. Settings > Add-ons > **Helm** (the local one) > **Restart**. It packs everything into that folder and
   stops (its Log says so).
4. Add-on Store: open **Helm** under "Helm" (the GitHub one) > **Install**, then **Start**. It takes the
   data, your settings and its Tailscale address from the local one (same address, same accounts) and
   deletes the packed copy. Settings you don't fill in again keep the old values.
5. Check Helm opens and your cards are there. Then **Uninstall** the local Helm, and switch **Auto update**
   on for the GitHub one if you like.

## While you're away

- **Using Helm**: Tailscale on, as at home. Your phone works from any network.
- **Is it running?** The still-alive alert above tells you when it isn't. Home Assistant's app (over
  Tailscale) shows Settings > Add-ons > Helm > **Log**, and has **Restart**.
- **Updates**: Settings > Add-ons > Helm > **Update** (GitHub install), or automatically.
- **What can't be fixed from far away**: a power cut longer than the machine's battery, or the home
  internet going down. The alert tells you; someone at home has to plug things back in.

## Backups

Home Assistant stops Helm for a moment during its own backup, so the copy is consistent. Helm also makes a
nightly backup into /data/backups, and a copy with secrets removed goes to
`\\homeassistant.local\share\helm-backups`.

## Import folder

Files dropped in `\\homeassistant.local\share\helm-import` are taken in by Helm the next time their owner
opens Home (for now: to-do lists for the To-dos module), then deleted. A file Helm can't read is renamed
`.failed`, with the reason next to it.

`about-me.md` (plain text) becomes what **your board** knows about you (Home > 🧭 Board > "What the board knows
about you", where you can also read and change it); Helm takes it in when the owner next opens the board, then
deletes the file.

## Calendar

Helm reads your calendars (read-only) to show a 📅 Calendar page and to let your board plan around your appointments.
No Google sign-in is needed: each calendar has a private address.

1. On a computer, open https://calendar.google.com > the gear (top right) > **Settings**.
2. On the left, under **Settings for my calendars**, click your calendar.
3. Scroll to **Integrate calendar** and copy **Secret address in iCal format** (ends with `basic.ics`).
4. Home Assistant > Settings > Add-ons > **Helm** > **Configuration** > **calendar_urls**: paste it. Several calendars:
   paste their addresses separated by a space. **Save**, then **Restart**.

Keep these addresses private: anyone who has one can read that calendar. If one leaks, Google's same screen has
**Reset**. Helm reads them at most every 15 minutes. Adding events from Helm will come later (switched off).

## Google Tasks

Helm can show your Google Tasks (due soon, overdue, or added lately) as cards, and all of them on a 📋 Google Tasks page.
It only **reads** them (it asks Google for read-only access). You make your own Google "OAuth client" once, so no
company sits between Helm and Google. Takes about 10 minutes, on the computer:

1. Open https://console.cloud.google.com and sign in with your Google account. Top left: **Select a project** >
   **New project** > name it `Helm` > **Create**, then select it.
2. Search bar: **Google Tasks API** > **Enable**.
3. Menu ☰ > **APIs & Services** > **OAuth consent screen** (may be called **Google Auth Platform**) > **Get started**:
   app name `Helm`, your email for support and contact, audience **External** > **Create**.
4. **Audience** > **Publish app** > **Confirm** (otherwise Google stops Helm's access after 7 days; nothing is made
   public: only accounts you sign in with can use it). If the button is greyed out: under **Test users** > **Add users**,
   add your own Google address instead, and press Connect again in Helm every 7 days.
5. **Clients** (or Credentials) > **Create client** > type **Web application**, name `Helm`. Under **Authorized redirect
   URIs** > **Add URI**: your Helm address followed by `/api/google/callback` (Helm shows the exact line in
   Settings > Google). **Create**.
6. Copy the **Client ID** and **Client secret** into Home Assistant > Settings > Add-ons > Helm > **Configuration**
   (`google_client_id`, `google_client_secret`) > **Save** > **Restart**.
7. In Helm: ⚙ Settings > **Google** > **Connect Google** > choose your account. Google warns "Google hasn't verified
   this app" (it's your own app): **Advanced** > **Go to Helm** > tick the access > **Continue**.

Disconnect any time in Settings > Google (Helm also tells Google to cancel its access). Backup copies that leave the
machine never contain the Google key.

## Your board and Help

**🧭 Board** (button on Home, and a card every morning from 07:00): your advisers say what matters today (three
things with a first step each, what can wait), and **Ask the board** helps with a decision (each adviser's view,
the options, what could go wrong, the chair's recommendation and one next step). Log what you decided; Helm asks how
it went on the day you chose. From 20:00, **Close the day with the board**: what you did (kept as your daily log),
what went well, what carries over, tomorrow's first step (then Tend's evening check-in). On Job search, **🪑 Ask the
board** on a job gives the board its details, and each application ready to send has an **📨 application kit**
(the posting, the CV and letter PDFs to upload, the prepared answers with Copy buttons). **🧪 Career paths**
(Work) suggests new directions from your profile, each with a small first experiment. The board uses Claude Sonnet when the Anthropic key is set (Settings > AI > "Your
board"), otherwise the default model.

**? Help** (top bar): frequent questions, the words Helm uses, and a helper that answers from this documentation.
It only explains; it never changes anything.

## Locked out?

- Forgot the password, or lost the phone AND the recovery codes: open the add-on's terminal (or the
  "Advanced SSH & Web Terminal" add-on), then run
  `docker exec -it addon_local_helm python -m app reset-password <username>` or `... reset-2fa <username>`.
- Tailscale broken: switch on **Allow the home network**, add a port (e.g. 8790) in the Network section of
  the Configuration tab, Save, Restart, and open `http://homeassistant.local:8790`. Switch it off again
  afterwards.
