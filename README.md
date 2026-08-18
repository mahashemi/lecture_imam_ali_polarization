# Talk Deck Framework — "The Power of Attraction in 'Ali"

A reusable Beamer (LaTeX slides) framework: content is separated from styling,
so future talks just need new `sections/*.tex` files.

## Folder contents

```
main.tex          <- edit title / author / which sections to include
talkstyle.sty      <- all styling + reusable commands (rarely needs editing)
sections/
  00_recap.tex
  01_opening.tex
  02_attractions.tex
  03_shiism.tex
  04_elixir.tex
  05_barriers.tex
  06_closing.tex
  07_quickref.tex
```

Each `sections/NN_name.tex` file contains ONE content slide followed by ONE
"Key Questions" slide built from the talk script / outline / source text.

## Compiling

You need a LaTeX distribution (MacTeX is the standard full install on macOS:
https://tug.org/mactex/). No extra packages beyond what ships with a normal
TeX Live install are required — this deck deliberately avoids exotic themes
(no metropolis, no tcolorbox) so it compiles anywhere.

```
cd talk_deck
pdflatex main.tex
pdflatex main.tex   # run twice so page numbers ("N / total") are correct
```

Or in TeXShop / Overleaf / VS Code+LaTeX Workshop: just build `main.tex`
normally (pdfLaTeX, XeLaTeX, and LuaLaTeX all work).

## How to edit / reuse this framework for a new talk

1. **Change the title page.** Edit the "EDIT THIS BLOCK" section at the top
   of `main.tex` (title, subtitle, author, date).

2. **Add/remove/reorder sections.** Add or remove `\input{sections/...}`
   lines in `main.tex`. Files are processed in the order listed.

3. **Write a new section file.** Copy an existing file in `sections/` as a
   template. Every section file follows this shape:

   ```latex
   \section{Short Name}          % shows up in outline navigation, if used

   \begin{frame}{Slide Headline}
     \bigquote{Quote text}{Attribution}
     \begin{itemize}
       \item Punchy bullet
     \end{itemize}
     \examplenote{Label}{Narrated story/example text}
     \delivernote{Interaction cue / "say aloud" note for the speaker}
   \end{frame}

   \qaframe{Section Title}{%
     \qa{Question text?}
       {Answer text.}
     \qa{Another question?}
       {Another answer.}
   }
   ```

   Available commands (all defined in `talkstyle.sty`):
   - `\bigquote{quote}{attribution}` — highlighted pull-quote box
   - `\examplenote{label}{text}` — tinted box for a narrated example/story
   - `\delivernote{text}` — small italic mic-cue line (interaction prompts,
     "say this aloud", timing reminders)
   - `\qaframe{Title}{ ... }` — a "Key Questions" slide. Put `\qa{Q}{A}`
     pairs inside. If the list is long it automatically overflows onto a
     second slide (same title) rather than shrinking or getting cut off.

   **Important:** `\qaframe` is a *command*, not an environment — always
   call it as `\qaframe{Title}{...}`, not `\begin{qaframe}...\end{qaframe}`.
   (Beamer's frame-body collection breaks if you split `\begin{frame}` and
   `\end{frame}` across a custom environment's begin/end hooks — this cost
   a round of debugging, so it's noted here to save you the trouble.)

4. **Re-theme the whole deck.** Edit the four color lines near the top of
   `talkstyle.sty` (`deepteal`, `gold`, `softgray`, `lightbg`). Everything
   else (quote boxes, example boxes, frame titles, footer) inherits from
   these automatically.

5. **Change the footer name.** Edit `\presentername` in `main.tex`.

## Notes on this specific deck

- Content is drawn from `slide_content.md` (the slide claims/bullets/
  examples), `talk_script_and_exam_questions.md` (the Key Questions +
  answers), and the outline's 🎤 interaction prompts (folded into
  `\delivernote{}` cues).
- Slide 4 (Elixir of Love) and slide 2 (Powerful Attractions) are the
  densest — they're already set at `\footnotesize` to fit comfortably.
  If you add more content to any slide, watch for "Overfull \vbox"
  warnings in the compile log — that's your signal to trim or shrink.
