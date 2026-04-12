# Codebook — `guerre_et_paix.csv`

## Overview

This dataset contains character-level linguistic annotations extracted from the French translation of *War and Peace* (*Guerre et Paix*) by Leo Tolstoy and from *Les Misérables* by Victor Hugo. Each row represents one character entity (co-reference chain) as it appears within a specific section of the text. The data captures the character's frequency of occurrence, inferred gender and number, and their syntactic roles (agent, patient, modifier, possessor) across the section.



---

## Column Descriptions


### `tome`
- **Description:** The internal book division.

---

### `chapitre`
- **Description:** The chapter within the part, encoded as a string label.

---

### `section`
- **Description:** A finer sub-division within a chapter, encoded as Roman numerals. Sections represent the smallest unit of text for which annotations are computed.

---

### `id`
- **Description:** The identifier of the character (co-reference chain) within its section. IDs are local to each section and restart from `0` in each new section. Characters are typically ranked by frequency of occurrence (id `0` being the most mentioned character in that section).

---

### `count`
- **Description:** A JSON-like dictionary containing the character's raw mention statistics within the section.
- **Keys:**
  - `occurrence` *(int)*: Total number of times the character is mentioned (across all mention types).
  - `mention_ratio` *(float)*: Share of this character's mentions out of all character mentions in the section.

---

### `gender`
- **Description:** Inferred gender of the character based on the mentions in the section.
- **Keys:**
  - `ratio` *(float)*: Proportion of mentions that were gendered (i.e., not gender-neutral).
  - `inference` *(dict)*: Probability distribution over `Male` and `Female`, derived from gendered mentions.
  - `max` *(float)*: Highest probability in the inference distribution.
  - `argmax` *(str)*: The predicted gender — either `"Male"` or `"Female"`.
- **Note:** A low `ratio` indicates most mentions were gender-neutral (e.g., pronouns like *vous*, titles), making the gender inference less reliable.

---

### `number`
- **Description:** Inferred grammatical number (singular vs. plural) of the character entity.
- **Keys:**
  - `ratio` *(float)*: Proportion of mentions that carried number information.
  - `inference` *(dict)*: Probability distribution over `Singular` and `Plural`.
  - `max` *(float)*: Highest probability in the inference distribution.
  - `argmax` *(str)*: The predicted number — either `"Singular"` or `"Plural"`.

---

### `mentions`
- **Description:** All surface forms (mentions) used to refer to this character in the section, organized by mention type.
- **Keys:**
  - `proper` *(list of dicts)*: Proper name mentions. Each entry has:
    - `n` *(str)*: The surface form.
    - `c` *(int)*: Count of occurrences.
  - `common` *(list of dicts)*: Common noun mentions (descriptive phrases, titles, relational nouns). Same `n`/`c` structure.
  - `pronoun` *(list of dicts)*: Pronominal mentions (personal and possessive pronouns). Same `n`/`c` structure.

---

### `agent`
- **Description:** Verbs for which this character is the grammatical subject (agent/actor). Captures what the character *does* in the section.
- **Structure:** A list of `{'w': <verb_lemma>, 'i': <token_index>}` entries, where `i` is the position of the token in the section's token sequence.

---

### `patient`
- **Description:** Verbs for which this character is the grammatical object (patient/recipient of action). Captures what is *done to* the character.
- **Structure:** Same as `agent` — a list of `{'w': <verb_lemma>, 'i': <token_index>}`.

---

### `mod`
- **Description:** Adjectives (and some nouns used as modifiers) that directly modify this character in the text. Captures how the character is *described*.
- **Structure:** A list of `{'w': <word_lemma>, 'i': <token_index>}`.

---

### `poss`
- **Description:** Nouns that appear in a possessive or genitive relation with this character (i.e., things *belonging to* or *associated with* the character). Captures the character's social and physical sphere.
- **Structure:** A list of `{'w': <noun_lemma>, 'i': <token_index>}`.

---

