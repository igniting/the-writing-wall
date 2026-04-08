The Writing Wall

Language models learned to read before they learned to write.

Not literally. But functionally, that’s where we are.

Today’s frontier models can ingest absurd amounts of text — hundreds of thousands, even millions of tokens — and reason over it. But ask them to produce something of comparable length, and they struggle. Not because they immediately fail, but because something subtle breaks: coherence, depth, structure, intent.

There is a wall. And it shows up much earlier than it should.

For a long time, we thought this was a systems problem. And part of it is. Prefill is parallel. Decode is sequential. KV cache grows. Bandwidth dominates. Generating 100k tokens is fundamentally expensive.

But that’s not the most interesting part.

The real story is simpler — and more surprising.

⸻

The wall wasn’t where we thought it was

In 2024, people noticed something strange.

No matter how you prompted them, models would stop writing around ~2,000 words. Ask for 10,000 — you’d get ~1,800. Ask for 20,000 — still ~1,900. It looked like a hard limit baked into the architecture.

It wasn’t.

LongWriter showed that the ceiling came from training data. Models were not unable to write longer — they had simply never seen long outputs during alignment. When researchers capped SFT data at different lengths, the model’s output length scaled almost linearly with that cap.

The “wall” was learned behavior.

That realization should have been humbling. For months, people searched for deep architectural explanations. The answer was sitting in the dataset.

But fixing the length didn’t fix the problem.

⸻

Long is easy. Non-hollow is hard.

Once you know the issue is data, the obvious move is to generate long outputs synthetically.

That works — sort of.

You can get models to write 10k, 20k, even 30k tokens. The outputs look structured. They follow outlines. They hit the right sections.

And yet, something feels off.

Every section looks the same. Transitions are mechanical. The writing has the shape of coherence, not the substance of it. It reads like something assembled, not something thought through.

This is the failure mode of most long-output pipelines today.

They optimize for structure, not sustained reasoning.

And that distinction turns out to matter more than anything else.

⸻

The real divide isn’t length. It’s verifiability.

Here is the key observation:

Some long outputs already work.

Reasoning models routinely produce tens of thousands of tokens — long chains of thought, proofs, code traces — and they hold together surprisingly well. Not perfectly, but much better than long-form writing.

Why?

Because they have something creative writing does not:

a verifier.

In math, the answer is either correct or not. In code, tests pass or fail. The reward signal is grounded. It does not degrade as the output gets longer.

That changes everything.

Now compare that to writing a 10,000-word essay. There is no oracle. You rely on reward models trained on human preference — and those models get worse as the input grows. They miss errors in the middle. They fail to capture global coherence. They reward style over substance.

So the long-output problem splits cleanly into two worlds:
	•	Verifiable long output — math, code, structured reasoning
	•	Non-verifiable long output — creative writing, open-ended prose

One is progressing rapidly. The other is stuck.

⸻

The category hiding in plain sight

There is a third category that doesn’t quite fit either bucket.

Translation.
Code migration.
Regulatory writing.
Citation-heavy documents.

These look like “writing tasks.” They produce long prose. But they are not purely subjective.

You can check whether a translation preserves entities and numbers.
You can check whether migrated code passes tests.
You can check whether citations actually support claims.

These are not perfect verifiers. But they are strong enough.

Call this class faithful-preservation tasks.

And this is where things get interesting.

Because once you have any programmatic signal that survives length, reinforcement learning starts to work again.

Recent systems like QwenLong-L1 and MemAgent are already exploiting this idea from adjacent directions: combining rule-based checks with learned rewards, training models to operate over long contexts with partial verification.

The pattern is emerging:

If you can check it, you can scale it.

⸻

The production workaround is not the solution

In the meantime, production systems have converged on a different answer.

Don’t generate long outputs.

Generate many short ones.

Plan → write → edit → verify.
Break the task into chunks.
Store intermediate state in files.
Use agents. Use tools. Use loops.

This works extremely well — which is why everyone ships it.

But it’s also a workaround.

The model is not actually getting better at long-form generation. The system is compensating for its weaknesses. Coherence is enforced by scaffolding, not learned behavior.

And you can see the cracks:
	•	Sections drift in tone
	•	Parallel edits conflict
	•	Global consistency is fragile
	•	Costs explode with scale

Most importantly, this approach only works when the task decomposes cleanly.

Which brings us back to the same fault line:

If correctness can be checked locally, chunking works.
If coherence must be global, it breaks.

⸻

Where the real opportunity is

The most promising direction is not “make models write longer.”

It is make long outputs checkable.

That shifts the problem from vague preference optimization to concrete signal design.

The playbook is straightforward, but underexplored:
	•	Pick a task where fidelity matters more than style
	•	Define the strongest programmatic checks you can
	•	Train with RL against those checks
	•	Scale length gradually

Translation with entity preservation.
Code migration with test suites.
Documents with citation validation.

These are not toy problems. They are exactly the kinds of things people want to use models for in production.

And they align perfectly with what RL is actually good at.

This is also why so much progress is happening in the open-weight ecosystem. Designing custom verifiers, iterating on reward signals, instrumenting failures — these workflows require control over the model.

Closed APIs are great for consumption. This problem is about construction.

⸻

What doesn’t change

Even if all of this works, some constraints remain stubborn.

Decode is still sequential.
KV cache still grows.
Bandwidth still matters.

You can push the wall back. You cannot remove it.

And some problems remain fundamentally hard:
	•	Neural text degeneration (the “repeat curse”)
	•	Context rot over long inputs
	•	Weak reward models for long-form prose
	•	Evaluation that doesn’t scale

These are not solved by better prompting or more data.

⸻

The writing wall

The long-output problem is not one frontier.

It is two.

One is already moving: tasks where correctness can be checked, even imperfectly. Here, RL works, systems improve, and products ship.

The other is still stuck: tasks where quality is subjective, coherence is global, and evaluation is fragile.

The mistake is treating them as the same.

The opportunity is realizing they are not.

And then asking a different question:

Not “how do we make models write longer?”
But “which long outputs can we actually verify?”

Because those are the ones that will scale first.