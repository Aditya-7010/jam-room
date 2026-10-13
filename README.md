# Jam Room – collaborative browser DAW
Static site, no build step.

## Deploy to Vercel
1. Push this folder to GitHub (or run `npx vercel` inside it).
2. Import the repo in Vercel; framework preset "Other"; leave build/output blank. Deploy.

## Real-time collaboration (Supabase, free tier)
Without this, the app syncs only between tabs of the same browser.
1. Create a project at supabase.com.
2. In the SQL editor run:
   create table rooms(id text primary key, data jsonb not null, updated_at timestamptz default now());
   alter table rooms enable row level security;
   create policy "open" on rooms for all using (true) with check (true);
3. Put your Project URL and anon key in `config.js`, redeploy.
4. Share a link such as https://your-site.vercel.app/#myroom. Everyone on the same #room edits together.

Anyone with the link can edit that room. Uploaded/recorded samples stay on each person's device.
Shortcuts: Space play/stop, Ctrl/Cmd+Z undo, Ctrl/Cmd+Y or Shift+Z redo.

## Daily-use features
- Record live: press Rec, then Play (1-bar count-in). Play with the computer keyboard (A W S E D F T G Y H U J K O L P ;, Z/X = octave, A–H = drum pads) or a USB MIDI keyboard (Chrome/Edge). Notes snap to 16ths.
- Key + scale highlighting with optional "lock" so you can only place in-key notes.
- Per-track volume, pan, tone, mute, solo; duplicate, rename, transpose, octave shift.
- Double bars (copies the whole song forward, up to 8 bars), swing, metronome, master volume + level meter.
- Room chat, invite-link button, undo/redo, WAV export, project save/load.

## v3 additions
- Select mode: drag a box or click notes (Shift adds), drag to move, Delete removes, [ ] resize, Ctrl+A/C/V select-all/copy/paste (pastes at last clicked step).
- Volume lane under the grid (per-step volume automation for the selected track; drag to draw).
- Per-track Delay and Reverb sends (the last two sliders on a track).
- Export MIDI (format 1, one track per instrument, drums on channel 10) and WAV for a single track.
- Samples (library files in `samples/`, uploads, mic recordings) are stored inside the project: saved in the project JSON and in the Supabase room row, so collaborators get them too. Uploads are converted to mono 22 kHz and trimmed to 6 s.
