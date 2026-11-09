# Camunda DMN: ILT Solutions

This repository contains the exercise solutions of the Camunda instructor-led training (ILT) *Camunda DMN*. The exercise instructions are published at https://camunda-academy.github.io/camunda-dmn-ilt/en/.

The solutions are DMN and BPMN files for Camunda 8.10. There is no code: you model the decisions in the Modeler of Camunda Hub and test them there.

---

## Business scenario

- **Exercise 0 and demo 0:** a *Dish* decision that picks a dish by season and vegetarian option.
- **Exercises 1 to 4:** a bank automates the granting of car loans. The *Car Loan Granting* DRD grows from one decision table (exercise 1) to a DRD with *Risk Level*, *Affordability* and the literal expression *Debt to Remaining Income Ratio* (exercise 3). Exercise 4 extracts the gateway logic of a BPMN process into the *Additional Background Check* decision.

---

## Repository structure

```
camunda-dmn-ilt-solutions/
└── assets/    # DMN decisions, BPMN test processes and test scenarios
```

### `assets/`

- **DMN diagrams**: the solution decisions (`*.dmn`)
- **BPMN diagrams**: the processes that call the decisions with a business rule task, to test them (`*.bpmn`)
- **Test scenarios**: test cases per process (`<process> test scenarios.json`), in the Camunda Process Test JSON format. Each case starts the process with the inputs from the exercise and asserts the result.

---

## Branch structure

Each branch contains the **solution of one exercise**, including everything from the exercises before it:

| Branch | Content |
| --- | --- |
| `main` | Base structure |
| `exercise-00` | [Exercise 0: Decide upon Dish](https://camunda-academy.github.io/camunda-dmn-ilt/en/exercise-0/), and the test process of [Demo 0: Execute your diagram](https://camunda-academy.github.io/camunda-dmn-ilt/en/demo-0/) |
| `exercise-01` | [Exercise 1: Car Loan Granting](https://camunda-academy.github.io/camunda-dmn-ilt/en/exercise-1/) |
| `exercise-02` | [Exercise 2: Categorize Credit Score as Risk Level](https://camunda-academy.github.io/camunda-dmn-ilt/en/exercise-2/) |
| `exercise-03` | [Exercise 3: Implement Affordability Decision](https://camunda-academy.github.io/camunda-dmn-ilt/en/exercise-3/) |
| `exercise-04` | [Exercise 4: Extract BPMN logic into DMN](https://camunda-academy.github.io/camunda-dmn-ilt/en/exercise-4/) |

To switch to the solution of a specific exercise:

```bash
git checkout exercise-NN
```

---

## Use a solution in Camunda Hub

1. In your project, click **Create new** → **Upload files** and upload the `.dmn` and `.bpmn` files of the branch.
2. Open the process, select the business rule task, open the **Test** tab in the **Details** panel, enter the input and click **Run test**. The test deploys the process together with the linked decision.
3. To run all test cases of a process at once, upload its `test scenarios.json` file too, and open the **Test** mode of the process. The file is linked to the process by its `processId`.


---

## This branch: exercise 1

Instructions: [Exercise 1: Car Loan Granting](https://camunda-academy.github.io/camunda-dmn-ilt/en/exercise-1/)

- `Car Loan Granting.dmn`: decision `carLoanGranting` with the inputs `creditScore` (number) and `affordability` (string) and the output `loanDecision` (`"Granted"` or `"Rejected"`).
  - Hit policy **First**: the two reject rules (credit score below 80, affordability `unaffordable`) overlap, so **Unique** is not possible with these rules.
  - The ranges follow the exercise (`[80..94.9]`). A credit score between 94.9 and 95, for example 94.95, matches no rule. Use `[80..95[` to close that gap.
  - The last rule is the optional part: any other affordability value is rejected (`not("affordable","marginal","unaffordable")`).
- `Exercise1-2-3Process.bpmn`: test process `Exercise1-2-3Process`, reused in exercises 2 and 3. The business rule task *Grant* calls `carLoanGranting` and stores the output in `result`.
