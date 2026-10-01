# Guide: how to work through this project

Think of this as a senior dev sitting next to you. It won't tell you the answers. It tells you
how to think, what to ask yourself, where people usually go wrong, and where to learn what you need.

---

## How to work, every single step

**1. Say what you want before you code.**
Write one sentence in a markdown cell first: "I want to know how many wafers have no defect."
If you can't write that sentence, you're not ready to code yet. You're ready to read.

**2. Guess the answer before you run it.**
"I expect about a third." Then run it. If you were wrong, that's the interesting part: why were you wrong?
That gap between what you expected and what happened is where you actually learn, and it's
exactly what your development steps document wants.

**3. Make it small first.**
Don't write 20 lines and run them. Write 2, run, look at the output, then add the next 2.
If something breaks you know it was the last 2 lines.

**4. Look at the data, not just numbers.**
A shape like `(38015, 52, 52)` tells you almost nothing. Plot a few. Print one. Your eyes find
things a summary hides.

**5. Be suspicious of good news.**
When something works the first time, or a score is very high, assume a mistake until you've
proven there isn't one. Bad scores make you check. Good scores make you stop checking. That's the trap.

**6. Keep a log while you work, not after.**
In C1 you rebuilt your steps from notes afterwards. Don't do that again. At the bottom of every notebook:
date, what you tried, what happened, what you decided. Three lines is enough.

---

## When you're stuck

Go through this in order. Most problems die at step 1 or 2.

1. **Read the whole error, bottom line first.** The last line says what went wrong. The lines above it say where.
2. **Print what you have.** `print(x.shape, x.dtype)` and `print(x[:3])` solve half of all numpy problems.
   You think it's one thing, it's actually another.
3. **Make it smaller.** Try the same thing on 5 wafers instead of 38,000.
4. **Read the docs page of the function.** Look at the examples at the bottom, they're usually the fastest way in.
5. **Search the error message** (without your own variable names).
6. **Ask.** A classmate, a teacher, or AI. Ask it to *explain* the error or the concept, not to fix your code.
   Keep the prompt for your AI usage section.

Rule of thumb: stuck for 20 minutes on the same thing, ask. Stuck for 5, keep going, that's normal.

---

## Phase 0: basics (before C2 starts)

**What you're doing:** getting enough Python, numpy and pandas that the tools don't slow you down.

**How to think about it:** you're not learning Python, you already program. You're learning
*a different way of working*: in numpy you almost never write loops. You do something to the whole
array at once. When you catch yourself writing a `for` loop over pixels, stop and ask
"how would I do this to the whole array at once?"

**Learn it here:**
- Kaggle Learn, *Python* (skim what you know) and *Pandas*: https://www.kaggle.com/learn
- *NumPy: the absolute basics for beginners*: https://numpy.org/doc/stable/user/absolute_beginners.html

**You're ready for phase 1 when** you can, without looking anything up: load an array, check its shape,
select a part of it, and count how often each value appears.

---

## Phase 1: get to know the data

**What you're doing:** finding out what you're actually working with, before you build anything.
Start a new notebook of your own.

**How to think about it:** pretend someone handed you this file and said "build a model on it by Friday".
A junior starts training. A senior first asks: *what is this, and can I trust it?*
Datasets from the internet are never as clean as the description says. Your job here is to find out
where it differs from what you were told.

**Questions to ask any dataset** (answer every one in your notebook, in your own words):
- How much data is there, and what does one example look like?
- What do the values mean? Does every value that appears match what the description says?
- What does the label look like, and what does it mean?
- How are the classes spread? Which are big, which are small?
- Is anything missing, strange, or there twice?
- Does what I see match the description (`data/` has the PDF that came with it)?

**Common traps:**
- Believing the description instead of checking it.
- Only looking at summaries, never at the actual pictures.
- Finding something weird and moving on. Write it down, even if you don't know what it means yet.

**Learn it here:**
- *Hands-On Machine Learning* (Géron), chapter 2, and the appendix *Machine Learning Project Checklist*
- The dataset's own description PDF

**You're ready for phase 2 when** you can explain the dataset to a classmate in two minutes,
including what surprised you.

---

## Phase 2: prepare the data

**What you're doing:** fixing what you found in phase 1, and splitting the data into parts.

**How to think about it:** the whole point of a model is to work on wafers it has *never seen*.
So you have to hide some wafers from it and only test on those at the end. Ask yourself:
- If my model has secretly seen a test wafer during training, what happens to my score? Is that score honest?
- Which of the things I found in phase 1 could cause that?
- Every class has to show up in every part. What if a class is small?
- Why do people use three parts instead of two? (Find out before you decide.)

**Every fix you make, write down why.** "I changed X to Y because Z." If you can't fill in Z, you're guessing.

**Common traps:**
- Fixing things *after* splitting instead of before.
- Looking at the test set while you're still improving the model. Once you've looked, it's not a test anymore.
- Not setting a random seed, so you get a different split every time and can't compare results.

**Learn it here:**
- Kaggle *Intro to Machine Learning*, lesson *Model Validation*
- Kaggle *Intermediate Machine Learning*, lesson *Data Leakage*
- Google *Machine Learning Crash Course*, the part on datasets, generalization and overfitting:
  https://developers.google.com/machine-learning/crash-course

**You're ready for phase 3 when** you can explain to your teacher why your test score will be honest.

---

## Phase 3: a simple baseline

**What you're doing:** building the dumbest model that could possibly work.

**How to think about it:** without a baseline, a score means nothing. "My CNN gets 90%"... is that good?
If a simple model gets 88%, your CNN barely helped. If it gets 40%, your CNN is great.
Ask yourself:
- What would a model score if it always guessed the biggest class? Calculate that first. That's your floor.
- What simple features could a human use to tell defects apart? (Think about where on the wafer the failures sit.)
- Can a simple model like a random forest learn from those?

**Common traps:**
- Skipping this because it's boring. It's the number your whole project gets measured against.
- Using accuracy only. Find out what accuracy hides when classes are uneven.

**Learn it here:**
- Kaggle *Intro to Machine Learning*, decision trees and random forest
- scikit-learn *Getting Started*: https://scikit-learn.org/stable/getting_started.html

**You're ready for phase 4 when** you have a baseline number and can say what it means.

---

## Phase 4: a neural network

**What you're doing:** building, training and improving your own CNN in PyTorch.

**How to think about it:** a neural network is a function with a lot of knobs. Training is turning
the knobs a tiny bit at a time so the output gets less wrong. Everything else is details.
Before you write it, make sure you can answer:
- What goes in (shape?), what comes out (how many numbers, what do they mean)?
- How does it know how wrong it is? (loss)
- How does it get less wrong? (optimizer)
- How do I know it's learning and not memorising? (training vs validation)

**How to build it:**
1. First make it **overfit on purpose** on a tiny piece of data, like 50 wafers. If it can't even
   memorise 50, something is broken. That's the fastest bug check there is.
2. Then train on everything and watch training *and* validation loss every epoch. Plot them.
3. Change **one thing at a time** and write down what changed. Change three things at once and you learn nothing.

**Common traps:**
- Shapes. Most PyTorch errors are shape errors. Print shapes everywhere.
- Forgetting to move the model *and* the data to the GPU.
- Training too long and not noticing validation getting worse.

**Learn it here:**
- PyTorch *Learn the Basics*, all chapters: https://pytorch.org/tutorials/beginner/basics/intro.html
- fast.ai *Practical Deep Learning for Coders*, lessons 1 and 2: https://course.fast.ai
- 3Blue1Brown, *Neural networks* video series on YouTube, for the intuition behind it

**You're ready for phase 5 when** your network beats the baseline on the validation set.

---

## Phase 5: evaluate properly

**What you're doing:** finding out *where* your model fails, not just how much.

**How to think about it:** one score hides everything interesting. Ask:
- Which classes does it get right, which wrong? Which ones does it mix up with each other, and why might that be?
- For a fab, what's worse: a bad wafer passed as good, or a good wafer sent to review? What does that mean for how you set your threshold?
- When the model is unsure, can it say so? What should the robot do then?
- Where on the wafer is the model actually looking? Is that where the defect is? (Grad-CAM)

**Only now** use your test set. Once.

**Learn it here:**
- scikit-learn *Metrics and scoring*, especially the confusion matrix, precision and recall:
  https://scikit-learn.org/stable/modules/model_evaluation.html
- `pytorch-grad-cam` on GitHub, read the README and examples

**You're ready for phase 6 when** you can say per class how good your model is, and why it fails where it fails.

---

## Phase 6: the robot cell

**What you're doing:** a simulated arm that moves each wafer to pass, review or scrap, based on your model.

**How to think about it:** this is your C1 experience again, so use it. Same lessons apply:
small steps, test each one. And think like a fab engineer:
- Does the arm need ML to move, or does it need ML to *decide*? What do real wafer handlers do?
- What are the fixed positions? What happens between "model says X" and "arm moves"?
- What can go wrong, and how does the cell notice?

**Build order, one at a time:** an empty scene that opens → an arm that moves to one point →
a wafer it can pick up → putting it somewhere → three destinations → your model deciding which.

**Learn it here:**
- MuJoCo documentation: https://mujoco.readthedocs.io (start with the *Overview* and the Python section)
- MuJoCo Menagerie, ready-made arms: https://github.com/google-deepmind/mujoco_menagerie

**You're ready for phase 7 when** a full cassette gets sorted without dropping a wafer.

---

## Phase 7: validate

**What you're doing:** testing the whole cell against your requirements and quality criteria.

**How to think about it:** same as C1. Same test, same way, every time, results saved automatically.
For every requirement ask: how would I prove this to someone who doesn't believe me?

**Learn it here:** the Fontys workshop slides for realisation and validation on Canvas.

---

## Before you start building

Ask your TC:
- Is MuJoCo OK instead of DobotLab?
- Does "ML decides, the arm executes" count as using machine learning to let a robot arm perform a task?

## Rules for yourself
- You write the code. AI may point you to where to learn something or explain an error. It doesn't write it.
- Keep your prompts for the AI usage section.
- No paths to your own laptop in the code.
