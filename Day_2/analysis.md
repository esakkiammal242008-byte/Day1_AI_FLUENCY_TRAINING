# Day 2 Lab Analysis

## Agentic AI: Foundations and Open-Source Practice

### Day 2: ReAct Tracing and Chain-of-Thought Comparison

---

## 1. Problem Statement

The given problem was to find which course combination is cheaper.

### Option 1
CS101 + AI202 with a 10% scholarship.

- CS101 = ₹12,000
- AI202 = ₹18,000
- Total = ₹30,000
- After 10% scholarship = ₹27,000

### Option 2
CS101 + AI202 + DS303 with a 25% scholarship.

- CS101 = ₹12,000
- AI202 = ₹18,000
- DS303 = ₹15,000
- Total = ₹45,000
- After 25% scholarship = ₹33,750

Therefore, **Option 1 is cheaper by ₹6,750.**

---

# 2. Part A – ReAct Trace on Paper

I first traced how the agent would solve the problem using the ReAct method.

| Step | Type | My Trace |
|---|---|---|
| 1 | Thought | I need the fees of all three courses. |
| 2 | Action | `get_course_fee("CS101")` |
| 3 | Observation | CS101 = ₹12,000 |
| 4 | Thought | Now I need the AI202 fee. |
| 5 | Action | `get_course_fee("AI202")` |
| 6 | Observation | AI202 = ₹18,000 |
| 7 | Thought | I also need the DS303 fee. |
| 8 | Action | `get_course_fee("DS303")` |
| 9 | Observation | DS303 = ₹15,000 |
| 10 | Thought | Now I can calculate the first option. |
| 11 | Action | `calculator("(12000+18000)*0.9")` |
| 12 | Observation | ₹27,000 |
| 13 | Thought | Now I can calculate the second option. |
| 14 | Action | `calculator("(12000+18000+15000)*0.75")` |
| 15 | Observation | ₹33,750 |
| 16 | Final Answer | The first option is cheaper by ₹6,750. |

### Answers

**How many tool calls were needed?**

I needed 5 tool calls: 3 course-fee lookups and 2 calculator calls.

**How many LLM calls would this involve?**

It would involve several reasoning/action cycles. The exact number depends on how the agent is implemented.

**Could any actions be done at the same time?**

Yes. The three course-fee lookups are independent, so they could theoretically be done in parallel.

---

# 3. Part B – ReAct Agent Output

I then ran the `react_trace.py` program.

The agent produced the following trace:

```text
QUESTION: Which is cheaper: CS101 and AI202 with a 10% scholarship, or all three courses with a 25% scholarship? By how much?

--- the agent's actions and observations ---
step 1: get_course_fee({'course_code': 'CS101'}) -> 12000
step 2: get_course_fee({'course_code': 'AI202'}) -> 18000
step 3: get_course_fee({'course_code': 'DS303'}) -> 15000
step 4: calculator({'expression': '(12000+18000)*0.9'}) -> 27000.0
step 5: calculator({'expression': '(12000+18000+15000)*0.75'}) -> 33750.0

FINAL ANSWER:
The combination of CS101 and AI202 with a 10% scholarship is cheaper.

Cost of CS101 + AI202 after 10% scholarship: ₹27,000
Cost of all three courses after 25% scholarship: ₹33,750
Difference: ₹6,750