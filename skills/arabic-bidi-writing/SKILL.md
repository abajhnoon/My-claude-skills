---
name: "arabic-bidi-writing"
description: "Use whenever writing Arabic mixed with English, numbers or symbols — in chat replies, Word, PowerPoint, Excel or HTML — so brackets, punctuation and terms don't flip."
---

# Arabic + English mixed text without bidi breakage

The flip happens when neutral characters (brackets, dots, colons, slashes, arrows, numbers) sit between or after text of different directions, or when a paragraph's direction or language tag is wrong. Fix it at the source of each format.

## Chat replies (Markdown)
- A paragraph's direction is set by its FIRST strong letter. Never start an Arabic sentence, bullet or heading with an English word; reorder so Arabic comes first.
- An English term at the END of an Arabic sentence followed by punctuation flips the punctuation. Either move the term earlier, or put it in inline code (`Term`).
- Prefer inline code for file names, paths, commands, model numbers and settings names (`Normal.dotm`, `Ctrl+S`).
- Put long English strings (commands, paths, code) on their own line or in a code block, never mid-sentence with brackets around them.
- Avoid wrapping English in parentheses inside Arabic text; use a dash or a colon before it instead, or put it in inline code.
- Menu paths made of English items: write the whole path as ONE inline-code span with RIGHT arrows, starting from the first menu: `File → Options → Language`. Neighbouring English words joined by arrows render as one left-to-right run, so left arrows (←) make the path read backwards. Never split the path across several code spans or mix Arabic words into it.

## Word (.docx)
- Any paragraph whose first letter is Arabic: paragraph bidi on (`<w:bidi/>`), right alignment.
- Arabic runs: `<w:rtl/>` and `<w:lang w:bidi="ar-SA"/>`; set the complex-script font (`w:cs`).
- English runs: `<w:lang w:val="en-US"/>`, no `w:rtl`.
- Brackets, punctuation and spaces that follow Arabic text belong in the Arabic (RTL) run, not the English run.
- Tables in Arabic documents: `<w:bidiVisual/>` on the table.
- If an English term ends a line before punctuation, insert U+200F (RLM) right after the term.

## PowerPoint (.pptx)
- Arabic paragraphs: `<a:pPr rtl="1" algn="r"/>`.
- Arabic runs: `<a:rPr lang="ar-SA"/>` with a `<a:cs>` font; English runs: `lang="en-US"`.
- Same neutral-character rule as Word: trailing brackets/punctuation go in the Arabic run.

## Excel (.xlsx)
- Sheet view right-to-left for Arabic sheets.
- Cells with Arabic text: alignment readingOrder = 2 (RTL), horizontal right.

## HTML / artifacts
- Arabic pages: `<html lang="ar" dir="rtl">`.
- Wrap every English fragment inside Arabic text in `<bdi>` (or `<span dir="ltr">`), including model numbers, file names and menu paths.
- Use CSS logical properties (`margin-inline-start`, `text-align: start`) instead of left/right.
- Numbers and code blocks: `dir="ltr"` on the element.

## Check before delivering
Render or open the output and look at every line that mixes scripts: brackets must close at the correct side, sentence-final punctuation must sit at the end of the Arabic sentence (left side), English terms must read left-to-right, and menu paths must start from the first menu.