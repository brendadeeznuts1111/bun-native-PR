# Color: Zero-Dependency, Native Color Management

> Parse, normalize, and convert colors across CSS, ANSI, numerical, and object formats with native Bun performance.

`Bun.color(input, outputFormat?)` is a powerful built-in utility that leverages Bun's highly optimized CSS parser to efficiently validate, normalize, and convert color representations. It offers comprehensive support for a wide array of color formats, making it an indispensable tool for frontend, backend, and CLI development.

| Format       | Output Example                       | Description                                                     |
| :----------- | :----------------------------------- | :-------------------------------------------------------------- |
| `"css"`      | `"red"`                              | Compact, valid CSS string, normalized for brevity.              |
| `"ansi"`     | `"\x1b[38;2;255;0;0m"`               | Auto-detects terminal capabilities for optimal ANSI output.     |
| `"ansi-16"`  | `"\x1b[38;5;9m"`                     | Approximates to the nearest of 16 standard ANSI colors.         |
| `"ansi-256"` | `"\x1b[38;5;196m"`                   | Approximates to the nearest of 256 standard ANSI colors.        |
| `"ansi-16m"` | `"\x1b[38;2;255;0;0m"`               | Full 24-bit TrueColor ANSI output (16 million colors).          |
| `"number"`   | `16711680` (0xFF0000)                | 24-bit integer representation (RGB).                            |
| `"rgb"`      | `"rgb(255, 99, 71)"`                 | Standard RGB function notation.                                 |
| `"rgba"`     | `"rgba(255, 99, 71, 0.5)"`           | Standard RGBA function notation.                                |
| `"hsl"`      | `"hsl(120, 50%, 50%)"`               | Standard HSL function notation.                                 |
| `"hex"`      | `"#1a2b3c"`                          | Lowercase 6-digit hex string.                                   |
| `"HEX"`      | `"#1A2B3C"`                          | Uppercase 6-digit hex string.                                   |
| `"{rgb}"`    | `{ r: 255, g: 99, b: 71 }`           | JavaScript object with `r`, `g`, `b` (0-255).                   |
| `"{rgba}"`   | `{ r: 255, g: 99, b: 71, a: 1.0 }`   | JavaScript object with `r`, `g`, `b` (0-255), `a` (0-1 float).  |
| `"[rgb]"`    | `[ 255, 99, 71 ]`                    | JavaScript array with `r`, `g`, `b` (0-255).                    |
| `"[rgba]"`   | `[ 255, 99, 71, 255 ]`               | JavaScript array with `r`, `g`, `b`, `a` (0-255 integer).       |

### **Key Applications & Advantages:**

*   **Universal Color Validation & Normalization:** Leverage Bun's robust CSS parser to accept virtually any CSS color input, instantly validating and standardizing it to your desired output format. This simplifies input handling dramatically.
*   **Optimal Data Persistence:** Convert colors to compact numerical representations (`"number"`) for efficient storage in databases or configuration files, minimizing data footprint.
*   **Rich & Adaptive Terminal Output:** Achieve sophisticated terminal styling with automatic adaptation to the user's terminal color capabilities (16, 256, or 16 million colors).
*   **Seamless Frontend Integration:** Generate perfectly formatted CSS strings for dynamic inline styles, CSS-in-JS libraries, or CSS variables, ensuring compatibility and payload efficiency.
*   **Powerful Design System Tooling:** Programmatically manipulate color components (e.g., `"{rgba}"`, `"{rgb}"`, `"[rgba]"`, `"[rgb]"`) for advanced theme generation, color palette creation, and dynamic UI adjustments.

`Bun.color` serves as a high-performance, **zero-dependency** native alternative to popular ecosystem packages like [`color`](https://github.com/Qix-/color) and [`tinycolor2`](https://github.com/bgrins/TinyColor), providing superior speed and direct integration within the Bun runtime.

### **Flexible Input Parsing:**

`Bun.color` accepts an extensive range of CSS-compliant color inputs, ensuring maximum flexibility for developers. This includes:

*   **Standard CSS Color Names:** e.g., `"red"`, `"papayawhip"`
*   **Hexadecimal Strings:** e.g., `"#f00"`, `"#ff0000"`, `"#1a2b3c"`
*   **RGB/RGBA Function Notations:** e.g., `"rgb(255, 0, 0)"`, `"rgba(255, 0, 0, 0.5)"`
*   **HSL/HSLA Function Notations:** e.g., `"hsl(0, 100%, 50%)"`, `"hsla(0, 100%, 50%, 0.7)"`
*   **Numerical Integers:** e.g., `0xff0000` (representing `0xRRGGBB`)
*   **JavaScript Objects:** e.g., `{ r: 255, g: 0, b: 0 }`, `{ r: 255, g: 0, b: 0, a: 1 }`
*   **JavaScript Arrays:** e.g., `[255, 0, 0]`, `[255, 0, 0, 255]`
*   **Modern CSS Color Spaces:** Includes advanced formats like `"lab(50% 50% 50%)"`, `"lch(50% 50 50)"`, `"oklab(50% 0 0)"`, providing future-proof parsing capabilities.

If the input cannot be parsed or is an invalid color, `Bun.color` will return `null`.

### **Output Format: `"css"` (Normalized CSS String)**

The `"css"` output format is designed for direct integration into web stylesheets, inline styles, or CSS-in-JS solutions. It intelligently **normalizes and compacts** the color input, returning the most succinct and valid CSS string possible without loss of color information. This includes converting `"#ff0000"` to `"red"` where a named color exists, or `"#aabbcc"` to `"#abc"` for shorthand hex values.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("red", "css");           // "red"
Bun.color(0xff0000, "css");        // "red" (Normalized to named color)
Bun.color("#f00", "css");          // "red"
Bun.color("rgb(255, 0, 0)", "css"); // "red"
Bun.color("rgba(255, 0, 0, 1)", "css"); // "red"
Bun.color("hsl(0, 100%, 50%)", "css"); // "red"
Bun.color("hsla(0, 100%, 50%, 0.5)", "css"); // "rgba(255, 0, 0, 0.5)" (Retains alpha)
Bun.color("#aabbcc", "css");       // "#abc" (Normalized to shorthand hex)
Bun.color("lab(50% 50% 50%)", "css"); // "lab(50% 50% 50%)" (Retains advanced CSS syntax)
```

### **Output Format: `"hsl"` (Human-Readable HSL String)**

The `"hsl"` output format provides a standard HSL (Hue, Saturation, Lightness) function string. This format is widely favored in **design systems and programmatic theme generation** due to its intuitive, human-centric nature. Designers and developers can easily predict and manipulate color variations (e.g., rotating hue, adjusting lightness for hover/active states) while maintaining perceptual consistency.

HSL's design system utility stems from its perceptual uniformity - changes to saturation and lightness feel natural and predictable, making it ideal for:
- **Theme generation**: Creating harmonious color palettes
- **Interactive states**: Consistent hover/focus/disabled variations
- **Accessibility**: Maintaining contrast ratios across color variations
- **Brand consistency**: Predictable color relationships

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("red", "hsl");           // "hsl(0, 100%, 50%)"
Bun.color("#00f", "hsl");          // "hsl(240, 100%, 50%)"
Bun.color("rgb(0, 128, 0)", "hsl"); // "hsl(120, 100%, 25%)"
Bun.color("rgba(128, 0, 128, 0.7)", "hsl"); // "hsla(300, 100%, 25%, 0.7)" (Retains alpha)
Bun.color(0x0000ff, "hsl");        // "hsl(240, 100%, 50%)"
Bun.color({ r: 255, g: 165, b: 0 }, "hsl"); // "hsl(39, 100%, 50%)" (Orange)
```

### **Output Format: `"ansi"` (Adaptive Terminal Colors)**

The `"ansi"` format generates ANSI escape codes for coloring terminal text. This output intelligently queries the current `stdout` environment to determine its color depth capabilities, automatically choosing the most suitable ANSI standard: `"ansi-16m"` (24-bit TrueColor), `"ansi-256"` (256-color), or `"ansi-16"` (16-color). If the terminal environment variables indicate no ANSI support, an empty string is returned, preventing garbled output.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("red", "ansi");           // "\u001b[38;2;255;0;0m" (Example output for 16m-capable terminal)
Bun.color(0xff0000, "ansi");        // "\u001b[38;2;255;0;0m"
Bun.color("navy", "ansi");          // "\u001b[38;2;0;0;128m"
```

#### **Specific ANSI Output Formats:**

*   **`"ansi-16m"` (24-bit TrueColor):** Outputs `\x1b[38;2;R;G;Bm` for terminals supporting 16 million colors. Direct RGB mapping.
    ```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
    Bun.color("red", "ansi-16m");   // "\x1b[38;2;255;0;0m"
    ```

*   **`"ansi-256"` (256-color approximation):** Approximates the input color to the nearest of the 256 standard ANSI colors. Bun utilizes a highly optimized algorithm, ported from `tmux`, for efficient and perceptually accurate color matching.
    ```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
    Bun.color("red", "ansi-256");   // "\u001b[38;5;196m"
    Bun.color("orange", "ansi-256"); // "\u001b[38;5;208m"
    ```

*   **`"ansi-16"` (16-color approximation):** Approximates the input color to the nearest of the 16 standard ANSI colors (8 basic colors + their bright variants). This ensures compatibility with the widest range of terminal emulators.
    ```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
    Bun.color("red", "ansi-16");    // "\u001b[38;5;9m" (Bright Red)
    Bun.color("navy", "ansi-16");   // "\u001b[38;5;4m" (Blue)
    Bun.color("gray", "ansi-16");   // "\u001b[38;5;7m" (White/Light Gray depending on background)
    ```

### **Output Format: `"number"` (Compact 24-bit Integer)**

The `"number"` format returns a 24-bit unsigned integer representing the RGB color (`0xRRGGBB`). This compact, numerical representation is ideal for:

*   **Database Storage:** Minimizing data size and simplifying indexing for color values.
*   **Configuration Files:** Storing colors in a portable and unambiguous format.
*   **Bitwise Operations:** Enabling efficient color manipulation at a low level.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("red", "number");        // 16711680 (0xFF0000)
Bun.color("blue", "number");       // 255 (0x0000FF)
Bun.color("papayawhip", "number"); // 16762363 (0xFFEFD5)
```

### **Output Format: Channel Access (`"{rgba}"`, `"[rgba]"` and variants)**

These formats provide direct access to the individual Red, Green, Blue, and Alpha channel components, crucial for programmatic manipulation and advanced color calculations.

#### **`"{rgba}"` Object Output:**

Returns an object `{ r: number, g: number, b: number, a: number }`. The `r`, `g`, `b` channels are integers from `0` to `255`, while the `a` (alpha) channel is a floating-point number from `0.0` to `1.0`, consistent with CSS `opacity` values.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
type RGBAObject = { r: number; g: number; b: number; a: number; };

Bun.color("hsl(0, 0%, 50%)", "{rgba}");     // { r: 128, g: 128, b: 128, a: 1.0 }
Bun.color("rgba(255, 0, 0, 0.5)", "{rgba}"); // { r: 255, g: 0, b: 0, a: 0.5 }
Bun.color("navy", "{rgba}");               // { r: 0, g: 0, b: 128, a: 1.0 }
```

The `"{rgb}"` format is identical but omits the `a` (alpha) channel, returning `{ r: number, g: number, b: number }`.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("rgba(0, 255, 0, 0.2)", "{rgb}"); // { r: 0, g: 255, b: 0 }
```

#### **`"[rgba]"` Array Output:**

Returns an array `[R, G, B, A]`. All channel values (R, G, B, A) are integers from `0` to `255`. This format is particularly useful for direct integration with `Uint8ClampedArray` or other typed array structures commonly used in graphics and image processing, where consistent integer ranges are preferred.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
type RGBAArray = [number, number, number, number];

Bun.color("hsl(0, 0%, 50%)", "[rgba]");     // [128, 128, 128, 255]
Bun.color("rgba(255, 0, 0, 0.5)", "[rgba]"); // [255, 0, 0, 128]
Bun.color("navy", "[rgba]");               // [0, 0, 128, 255]
```

The `"[rgb]"` format is identical but omits the `a` (alpha) channel, returning `[R, G, B]`.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("rgba(0, 255, 0, 0.2)", "[rgb]"); // [0, 255, 0]
```

### **Output Format: Hex Strings (`"hex"`, `"HEX"`)**

The `"hex"` and `"HEX"` formats output standard hexadecimal color strings, prefixed with `#`. `"hex"` provides a lowercase output (`#rrggbb`), while `"HEX"` provides an uppercase output (`#RRGGBB`). These are commonly used in web contexts and various configuration files.

```ts theme={"theme":{"light":"github-light","dark":"dracula"}}
Bun.color("hsl(0, 0%, 50%)", "hex"); // "#808080"
Bun.color("red", "hex");             // "#ff0000"
Bun.color("blue", "HEX");            // "#0000FF"
Bun.color(0x00ff00, "HEX");          // "#00FF00"
```

### **Best Practices & Considerations:**

*   **For Database Storage:** Prefer `"number"` for its minimal storage footprint and direct numerical representation.
*   **For Web CSS:** Use `"css"` for normalized, compact output, or `"rgb"`, `"rgba"`, `"hsl"`, `"hsla"` for more human-readable or manipulatable programmatic CSS.
*   **For Terminal Logging:** Use `"ansi"` for automatic terminal compatibility, or explicitly `"ansi-16m"` for TrueColor if you know the environment supports it for highest fidelity.
*   **For Frontend Bundles:** Leverage **Bundle-time client-side color formatting** via macros to pre-compute colors, eliminating runtime parsing/conversion and reducing client-side JavaScript payload.

### **Bundle-time Client-Side Color Formatting (Macros)**

One of `Bun.color`'s most powerful features for web development is its integration with Bun's macro system. This allows `Bun.color` invocations to be evaluated and resolved *at bundle-time*, during the build process, rather than at runtime in the browser.

This means you can declare colors using any flexible input format in your client-side source code, and Bun's bundler will replace the `Bun.color()` call with the final, compiled color string (e.g., `"red"`) directly into the JavaScript output. This results in:

*   **Zero Client-Side Runtime Cost:** No `Bun.color` code is shipped to the browser.
*   **Smaller Bundles:** Reduced JavaScript payload size.
*   **Faster Execution:** No runtime parsing or conversion logic needs to be executed in the browser.

To use `Bun.color` as a macro, import `color` from `"bun"` with the `type: "macro"` import attribute:

```ts client-side.ts theme={"theme":{"light":"github-light","dark":"dracula"}}
import { color } from "bun" with { type: "macro" };

// These calls are resolved during the `bun build` process.
console.log(color("#f00", "css"));
console.log(color("hsl(240, 100%, 50%)", "hex"));
console.log(color("rgba(0, 255, 0, 0.75)", "{rgba}")); // Even object outputs can be stringified if necessary, or transformed into structured data.
```

Then, compile your client-side code with Bun's bundler:

```sh theme={"theme":{"light":"github-light","dark":"dracula"}}
bun build ./client-side.ts --outfile ./dist/client-side.js
```

The resulting `dist/client-side.js` will contain the pre-computed color values:

```js theme={"theme":{"light":"github-light","dark":"dracula"}}
// dist/client-side.js (Output after bundling)
console.log("red");
console.log("#0000ff");
console.log({ r: 0, g: 255, b: 0, a: 0.75 }); // Object literals are preserved
```
