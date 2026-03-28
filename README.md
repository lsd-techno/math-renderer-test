# Math Rendering Test Corpus

This README is a renderer-compatibility corpus for Markdown math.
It is intended to compare how GitHub, Gitea, VS Code Markdown Preview, Pandoc, MkDocs, and similar engines handle:

- Inline math
- Display math
- Different delimiter styles
- Punctuation next to math
- Mixed Latin and CJK text around math
- Known edge cases that are valid in some engines and broken in others

The examples are grouped by portability so that failures are easier to interpret.

## 1. Broadly Portable Inline Math

These are the safest tests for TeX-aware Markdown renderers.

- Simple inline: $E = mc^2$
- Inline with trailing comma: $E = mc^2$, followed by English text.
- Inline with trailing period: $E = mc^2$.
- Inline before ASCII colon: $E = mc^2$: English text after.
- Inline before Chinese full-width colon: $E = mc^2$：中文文字紧跟
- Inline before Japanese text: $E = mc^2$：日本語のテキスト
- Inline before Korean text: $E = mc^2$：한국어 텍스트
- Fraction: $\frac{a}{b}$
- Square root: $\sqrt{x^2 + y^2}$
- Subscript: $H_2O$
- Superscript: $x^2$
- Combined subscript and superscript: $x_i^2$
- Greek letters: $\alpha + \beta = \gamma$
- Summation: $\sum_{i=1}^{n} i$

## 2. Broadly Portable Display Math

These use `$$...$$`, which is common but still not universal across all Markdown engines.

Simple display math:

$$
E = mc^2
$$

Pythagorean theorem:

$$
a^2 + b^2 = c^2
$$

中文说明：勾股定理

Quadratic formula:

$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

Integral:

$$
\int_0^1 x^2\,dx = \frac{1}{3}
$$

Matrix:

$$
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
$$

## 3. Mixed Paragraph Tests

In a normal paragraph, inline math should still work: The energy-mass equivalence is $E = mc^2$.

中文句子中内联公式 $E = mc^2$ 应该正常渲染。

Another paragraph with a full-width colon right after the formula: $E = mc^2$：测试中文冒号。

Another paragraph with parentheses around the formula: ($E = mc^2$) should remain inline.

## 4. Delimiter Variant Tests

These are useful because engines differ on which math delimiters they recognize.

- TeX inline delimiters: \(E = mc^2\)
- TeX display delimiters:

\[
E = mc^2
\]

Fenced math block:

```math
E = mc^2
```

## 5. Renderer-Sensitive Formula Cases

These are still formula tests, but real-world renderers often disagree on them.
They should not automatically be treated as invalid input, because some engines render them correctly and others fall back to plain text or partially broken output.

- Full-width punctuation with an added space: $E = mc^2$ ：中文，加空格
- Backticks inside math delimiters: $`E = mc^2`$

## 6. Escaped Dollar And Currency Tests

These are not math formulas. They should render as literal dollar signs in normal text.

- Escaped dollar sign: \$5 should remain plain text, not math.
- Escaped currency in a sentence: The total is \$5.
- Unescaped currency source text example: `The total is $5.`
- Escaped currency source text example: `The total is \$5.`

## 7. Interpretation Notes

- If section 1 fails, the renderer likely has weak or disabled inline math support.
- If section 2 fails but section 1 works, the renderer likely supports inline math but not block math.
- If section 4 fails while sections 1 and 2 work, the renderer likely supports only one delimiter family.
- If section 5 fails, that is a compatibility limitation worth noting, not necessarily a malformed test case.
- Section 6 should render as ordinary text. If it triggers math parsing, dollar escaping is likely mishandled.
