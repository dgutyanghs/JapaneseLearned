# 🇯🇵 Japanese Grammar Notes: i-Adjectives vs. na-Adjectives

---

## 📌 Core Architecture Overview

Japanese adjectives are split into two core paradigms. Think of them as two different UI components that accept different props and have distinct state-management lifecycles.

| Property / Feature | `i-Adjective` (い形容詞) | `na-Adjective` (な形容詞) |
| :--- | :--- | :--- |
| **Naming Convention** | Morphological Adjectives | Nominal Adjectives (Noun-like) |
| **String Ending** | Must end in a trailing **「い」** | Usually ends in Kanji/Kana **(Not 「い」)** |
| **Component Hook (Noun Mod)**| `[Word] + [Noun]` (Direct) | `[Word] + な + [Noun]` |
| **State Lifecycle (Inflection)**| Mutates the trailing `い` itself | Mutates the trailing auxiliary `だ / です` |
| **Data Structure Dependency** | Independent grammar logic | Inherits 100% from **Noun (名詞)** syntax |

---

## 🚨 Edge Cases & Exceptions (The "Linter Errors")

These words end in the phonetic sound `い` but are **statically typed as `na-Adjectives`**. This is because the `い` is part of the Kanji's On'yomi reading or the immutable root.

*   `きれい` (Beautiful/Clean) ➔ `きれいな`
*   `有名 (ゆうめい)` (Famous) ➔ `有名な`
*   `嫌い (きらい)` (Dislike/Hate) ➔ `嫌いな`
*   `得意 (とくい)` (Good at) ➔ `得意な`

---

## 🛠️ State Inflection Tables (Conditional Rendering)

### 1. `i-Adjective` Lifecycle (Mutates the `い` suffix)
*Example Base Word: **高い (たかい - Expensive / High)***

| State / Tense | Formal (敬体 / です) | Informal (簡体 / だ) | Morphological Mutation |
| :--- | :--- | :--- | :--- |
| **Present Affirmative** | 高い**基础**です | 高い | *No change* |
| **Present Negative** | 高**基础**くないです | 高**基础**くない | `い` ➔ `くない` |
| **Past Affirmative** | 高**基础**かったです | 高**基础**かった | `い` ➔ `かった` |
| **Past Negative** | 高**基础**くなかったです | 高**基础**くなかった | `い` ➔ `くなかった` |

⚠️ **Global Override Exception:** The word **「いい」 (Good)** must fall back to its legacy root **「よい」** before updating state:
*   `いい` ➔ `よくない` (Not good) ➔ `よかった` (Was good) ➔ `よくなかった` (Was not good).

### 2. `na-Adjective` Lifecycle (Immutable Base + Appended State)
*Example Base Word: **静か (しずか - Quiet)***

| State / Tense | Formal (敬体 / です) | Informal (簡体 / だ) | Morphological Mutation |
| :--- | :--- | :--- | :--- |
| **Present Affirmative** | 静かです | 静かだ | Base + `です / だ` |
| **Present Negative** | 静かじゃありません | 静かじゃない | Base + `じゃありません / じゃない` |
| **Past Affirmative** | 静かでした | 静かだった | Base + `でした / だった` |
| **Past Negative** | 静かじゃありませんでした | 静かじゃなかった | Base + `じゃありませんでした / じゃなかった` |

---

## 🚀 Advanced API: Method Chaining & Side Effects

When you need to pipe adjectives together (Concatenation) or pass them as parameters to verbs (Adverbial transformation).

### A. Adjective Conjunction (The `&&` Operator)
To chain multiple adjectives together (e.g., "Cheap *and* delicious"):

*   **`i-Adjective`**: Swap `い` for **`くて`**
    *   *Syntax:* `安い (Cheap)` + `美味しい (Delicious)` ➔ **安くて**、美味しいです。
*   **`na-Adjective`**: Append **`で`** to the base root.
    *   *Syntax:* `静か (Quiet)` + `きれい (Clean)` ➔ **静かで**、きれいです。

### B. Adverbial Conversion (Type Casting to Adverb)
To modify a verb (e.g., "Run *quickly*", "Play *quietly*"):

*   **`i-Adjective`**: Swap `い` for **`く`**
    *   *Syntax:* `早い (Fast)` + `走る (Run)` ➔ **早く**走る。
*   **`na-Adjective`**: Append **`に`** to the base root.
    *   *Syntax:* `静か (Quiet)` + `座る (Sit)` ➔ **静かに**座る。
