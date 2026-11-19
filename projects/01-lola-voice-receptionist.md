# Lola | A bilingual phone receptionist for psychiatry

I built Lola for Elevate Psychiatry. She handles English and Spanish calls about appointments, refills and provider messages, with transfers to the front desk when a human needs to take over. RingCentral connects the call, local models handle speech and language, and tools interact with athenaOne. The live configuration documented on November 18 uses Qwen 3.8 27B. The latest simulator and workflow changes described below are in development.

## From a phone call to a testable outcome

A patient who hears “you’re booked for Tuesday” expects to find an appointment on Tuesday. Lola’s booking workflow verifies identity, offers an available slot, reads it back, and asks for confirmation. A check on the outgoing speech catches a booking claim without a successful tool result. Testing a simulated call means inspecting the conversation, the tools it used and the resulting mock appointment record.

## Why I paused fine-tuning

The November 1 benchmark replayed scenarios derived from 2,514 real front-desk calls. Lola passed 1,509 (60%). English calls passed at 62%, Spanish at 46%, and mixed-language calls at 47%. These figures describe simulated calls under that version of the evaluation.

I used paired runs to test changes. One fine-tuned model moved from 348 to 359 passes on a 497-call comparison (p = 0.19). Another moved from 346 to 347. Repeating the same code could flip the outcome on 83–110 of 497 calls. I paused training to investigate what the score was measuring.

| November 18 audit of 30 failed calls | Calls |
|---|---:|
| Test alone caused the failure | 19 |
| Lola alone caused the failure | 5 |
| Both contributed | 6 |

The simulated caller sometimes repeated words from the original call after Lola had asked a different question. The grader sometimes expected the exact action the human receptionist had taken, even when practice policy allowed another outcome. I ruled on seven ambiguous outcomes. We updated the grader and rebuilt the caller to respond to the conversation in front of it. A simulation that repeatedly breaks its own rules is now marked invalid.

## A fix we could isolate

Call transfers could stall, and spelling out a name with pauses could derail the conversation. In a targeted development test of 36 scenarios run twice, transfer and spelling fixes raised the pass rate from 26% to 83%. Deadlocks fell from 22 to one. We selected these cases because they exercised the bugs. The broader paired evaluation and safety checks are separate release criteria.

## What runs where

Speech recognition, the language model and speech synthesis run locally. Python enforces caller verification and tool permissions. Crisis routing can bypass the model, and staff can take over the call. The evaluation uses de-identified scenarios, simulated callers and mock records. The nightly development process compares candidate changes on paired calls and checks for regressions in crisis handling before a release decision.
