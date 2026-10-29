---
layout: post
title: "AI fluency means knowing when to trust the answer"
date: 2026-10-29
series: "Working with AI agents"
takeaways:
  - An AI chat does not look answers up. It predicts the next word from what it learned and what you gave it.
  - Because of that, a wrong answer sounds exactly as confident as a right one.
  - Its strengths and weaknesses come from the same four properties, so you can predict where it will fail.
  - Four questions before you use an answer - Delegate, Describe, Evaluate, Verify. Verify is the one people skip.
discuss:
  linkedin: https://www.linkedin.com/posts/alexandru-diacov_ai-fluency-means-knowing-when-to-trust-the-share-7521471237156446208-M6E0/
---

When I first used an AI chat, it felt like magic. I could ask about anything and get an answer that sounded clear, confident and human. Then I started using it for real work, and the problems showed up. Some answers were simply wrong. Others were invented, delivered in the same calm, expert tone as the correct ones. After enough of that I stopped using it. If I could not tell the good answers from the bad ones, every answer needed checking, and then what was the point?

Some time later I got curious and wanted to know what actually happens when the AI gets something wrong. It turned out the mistakes were not random at all. Once I understood how the thing works, I could predict where it would fail, and that changed how I use it. I later turned what I learned into a short course, and this article is the written version, without the technical jargon. That, for me, is what AI fluency means. It is not about knowing more prompts, it is about knowing when to trust the answer and when to check it.

## What happens when you ask a question

Classic software follows rules a person wrote. If your request does not match a rule, you get an error. A large language model (LLM, the engine inside every AI chat and coding agent) has no such rules. It learned from an enormous amount of text, and from that text it learned how words tend to follow each other.

When you type a question, the model first splits your text into small pieces called tokens, roughly one word each, and turns every piece into a number. From that point on it works with numbers, not words. Then it does one single thing. It calculates which word is most likely to come next.

![The sentence "The weather today is" with two possible next words: sunny at 95% and blue at less than 1%](/assets/images/ai-fluency/next-word.webp)

Take "The weather today is ___". "Sunny" is a very likely next word. "Blue" is possible, but rare. The model does not pick "sunny" because it understands weather. It picks it because in everything it read, "sunny" followed sentences like this far more often. The chosen word is added to the sentence, and the model repeats the calculation for the next word until the answer looks complete. There is no finished paragraph stored anywhere. The answer is built one word at a time, as it goes.

![The sentence is split into tokens, each turned into a number. The LLM picks the most likely next word, sunny, which is added back to the sentence before the next step](/assets/images/ai-fluency/loop.webp)

What is the guess based on? Only two things: what the model learned in training, and what is in front of it right now. The second part is called the context. It is your question, any document you attached, and the conversation so far. The model does not know who you are, which meeting you mean, or what your team decided last week, unless that is in the context.

![What the AI can see: your question, an attached document and the chat so far. What it cannot see: who you are, which meeting you mean, what your team decided last week](/assets/images/ai-fluency/context.webp)

That is the whole mental model. The AI does not look the answer up, it predicts it. Most of the time the prediction is very good. Sometimes it is fluent, confident and wrong, and from the outside both cases look the same.

## One property, two sides

Think about the sun. The same light that keeps everything alive will burn you if you stay in it too long. The properties of an AI work the same way. Each one is a strength in one situation and a weakness in another, and I found four worth knowing.

**Prediction.** On topics the model has seen thousands of times, the prediction is excellent. That is why it is good at first drafts, summaries, rewriting a text in a different tone, and explaining common ideas. On a fact it never learned, it does not stop and say "I don't know". It keeps predicting words that sound right. This is what people call a hallucination, a fluent and logical answer that happens to be false. The AI companies are working on it, and sometimes the model does admit it is unsure, but not reliably enough to count on.

**Knowledge.** The model knows what was written up to the moment its training stopped, and nothing after. It is strong on topics with a lot of text behind them, like general science or popular programming languages, and weak on recent events and niche subjects. So the useful question is not "does the AI know this?" but "how much has been written about this?". For your company's internal tools, the answer is close to zero.

**Working memory.** The context has a fixed size, measured in tokens. Today that ranges from about one book to several books, depending on the model. When everything fits, the model uses all of it well. In a very long conversation the space fills up and older details drop out. The model keeps answering just as confidently, but it no longer sees what you said at the start, and the answers slowly drift. When a long chat starts acting strange, ask for a summary and start a new chat from it.

**Steering.** On top of learning text, the model was trained to be a helpful assistant, so you can steer it with instructions about role, tone, format and length. But it responds to the words you wrote, not to what you meant. "Answer in at most 100 words, as a table" works well. "Make it better" forces the model to guess what better means, and its guess may not be yours. Instructions that contradict each other during a conversation cause the same problem.

None of these are bugs that the next version will fix. They come from how the system works, so they apply to every tool built this way, today and next year.

## Four questions I ask every time

In daily work I do not want to think about tokens, I want a short routine. The one I use comes from Anthropic's AI Fluency course, and it is four questions.

![A checklist with Delegate, Describe and Evaluate ticked, and Verify left empty as the step people skip](/assets/images/ai-fluency/four-questions.webp)

**Delegate: who should do this, me or the AI?** Good candidates are low stakes and easy to check, like a first draft of an email, a summary of a 50-page report, or a list of ideas. You read the result and immediately see whether it is good. Bad candidates are decisions about people, money or strategy, such as a performance review, a conflict in the team or a hiring decision. The AI only knows what you told it, not the history between people or what was never written down. It also does not carry the responsibility. It will always write something convincing on these topics, and that is exactly the trap. Use it to think, but keep the decision.

**Describe: did I give it everything it needs?** If there is one skill worth practicing, it is this one. Compare two requests:

> Write a message about the meeting change.

> Write a short message (100-150 words) for the backend team. Monday's planning meeting moves to Wednesday at 10:00 because it overlaps with the architecture review. Use a friendly but direct tone.

The first one gets you a generic text you will rewrite from scratch. The second is almost ready to send. The difference is context (who reads it, what changed, why) and specifics (length, format, tone). Also, treat the first answer as a draft. Asking for "shorter" or "more formal" is normal, not a failure.

**Evaluate: is it written well?** Read the answer like an editor. Is it clear, is the structure logical, is the tone right for the reader?

**Verify: is it true?** Check every number, date, name and source against the original. This step and the previous one sound similar, but they catch different problems. Evaluation tells you the text sounds good. Verification tells you it is correct. A hallucination passes the first check easily, because it is written just as well as the truth.

## What skipping a step looks like

None of the mistakes below looked like a mistake at the time.

A backend engineer needs documentation for a new internal API endpoint. It is a good task for the AI, much better than formatting Markdown by hand (the security logic itself is not something to delegate). They attach the code, say the readers are the frontend team, and ask for JSON request and response examples. The result reads well, so it goes straight into the wiki. But the AI added a `timeout` parameter that does not exist in the code, listed a 404 error the endpoint never returns, and mixed up 401 and 403. All of it looked plausible, because endpoints like this often have such things. A few weeks later the frontend team builds against the documentation, and the release breaks in production. Comparing the parameters and status codes with the code would have caught all of it.

![Generated API documentation with the invented timeout parameter and a 404 error the endpoint never returns circled](/assets/images/ai-fluency/verify.webp)

An HR colleague asks for a job ad for a DevOps role, based on the hiring manager's notes about AWS, Kubernetes and Terraform. The ad comes back professional and attractive, and it also lists Puppet and Ansible as required. The team does not use them. They were added because these tools often appear together online. Some strong candidates read the list, decide they do not qualify, and never apply. Nobody ever finds out who they were.

A team manager prepares notes for a meeting about a project reorganization. The AI structures the agenda nicely, but the tone is cold and full of corporate phrases, and one project name and two dates are wrong. Short on time, the manager reads the notes almost word for word, and someone in the team corrects the dates in front of everyone. The mistake was small, but the room now trusts the reorganization a bit less, and the manager too. Here two steps were skipped. Evaluate would have fixed the tone, and Verify the facts.

## Before you press send

The AI predicts the next word from what it learned and what you gave it. It does not look anything up, and it sounds equally sure when it is right and when it is wrong. That will not change with the next model, so the work does not end when the answer appears.

Delegate, Describe, Evaluate, Verify. Of the four, Verify is where most of the damage happens, and it is also the easiest one to skip, because by then the answer already looks finished.

---

*This article is based on my AI Fluency course, adapted from the Anthropic Academy courses [AI Fluency: Framework & Foundations](https://anthropic.skilljar.com/ai-fluency-framework-foundations) (Rick Dakan, Joseph Feller and Anthropic) and [AI Capabilities and Limitations](https://anthropic.skilljar.com/ai-capabilities-and-limitations). Like the originals, it is shared under the [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) license.*
