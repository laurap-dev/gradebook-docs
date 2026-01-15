## Special Import codes for Clasroom Import

Import needs to check assignment instruction section for any codes but based on the gradebook types which are already defined and available in the process:
- `isTotalPointsGradeBook`
- `isStandardGradeBook` 
- `isCategoryGradeBook`
- `isTermsGradeBook`

## Task:  Ensure all of these are implemented corrected by doind a deep review of the import process start from `import/start/start.js` and summarize here:

## Case 1: Category GradeBook
### Automatic Categories and Weighting – Category GradeBook

If your GradeBook type is **Category** or **Category with Terms**, you can have GradeBook automatically set the assignment category and weight when you import from Google Classroom.

| GradeBook type      | Code                    | Example              |
|---------------------|-------------------------|----------------------|
| Category            | `[[category, weight]]`  | `[[Tests, 20]]`      |
| Category with Terms | `[[category, weight, term]]` | `[[Tests, 20, Term]]` |

When you import, GradeBook will set the assignment category and weight based on the values in the code. The **category** (and **term**, if used) must match **exactly** the names in your **Settings** sheet.

## Case 2: Standard GradeBook
### Automatic Weighting – Standard GradeBook    

If your GradeBook type is **Standard** or **Standard with Terms**, you can have GradeBook automatically set the assignment weight.

| GradeBook type      | Code               | Example           |
|---------------------|--------------------|-------------------|
| Standard            | `[[weight]]`       | `[[20]]`          |
| Standard with Terms | `[[weight, term]]` | `[[20, Term 1]]`  |

When you import, GradeBook will set the assignment weight based on the number you include in the code. If you include a term, its name must match **exactly** the term name in your **Settings** sheet.

## Case 3: Any GradeBook Type
### Stop an Assignment from Being Imported

If there is an assignment in Google Classroom that you do **not** want imported into GradeBook, add this code to the instructions:

| GradeBook type | Code          | Example        |
|----------------|---------------|----------------|
| All            | `[[ungraded]]`| `[[ungraded]]` |

When you import from Google Classroom, any assignment whose instructions contain `[[ungraded]]` will be skipped and **not** imported into GradeBook.

## Summary of required implementation cases

The special import codes above imply six distinct behavior cases that the import process must handle correctly:

1. **Category, no terms**  
   - Conditions: `isCategoryGradeBook === true` and `isTermsGradeBook === false`.  
   - Code: `[[category, weight]]`.  
   - Behavior: set the assignment category and weight from the two values in the code.

2. **Category with terms**  
   - Conditions: `isCategoryGradeBook === true` and `isTermsGradeBook === true`.  
   - Code: `[[category, weight, term]]`.  
   - Behavior: set assignment category, weight, and term using the three values; category and term names must exactly match the Settings sheet.

3. **Standard, no terms**  
   - Conditions: `isStandardGradeBook === true` and `isTermsGradeBook === false`.  
   - Code: `[[weight]]`.  
   - Behavior: set the assignment weight from the single value in the code.

4. **Standard with terms**  
   - Conditions: `isStandardGradeBook === true` and `isTermsGradeBook === true`.  
   - Code: `[[weight, term]]`.  
   - Behavior: set assignment weight and term using the values in the code; term name must exactly match the Settings sheet.

5. **Skip import for a specific assignment (any GradeBook type)**  
   - Conditions: any combination of the four GradeBook flags (`isTotalPointsGradeBook`, `isStandardGradeBook`, `isCategoryGradeBook`, `isTermsGradeBook`).  
   - Code: `[[ungraded]]`.  
   - Behavior: do not import that Classroom assignment into the GradeBook at all.

6. **No recognized special code present**  
   - Conditions: any GradeBook type and instructions text that does not contain any of the patterns above.  
   - Behavior: import the assignment normally, using the existing GradeBook configuration (categories, weights, and terms) without any overrides from the instructions text.