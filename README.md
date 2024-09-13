# Cadastral Take Home

👋 Hey, welcome! We’re excited you’re interested in the AI Engineering role at Cadastral. Your task is to help us pick the best OCR provider.

## Goal

In this take-home exercise you will experiment with different OCR providers and evaluate them for accuracy and speed. You will implement and evaluate [Tesseract](https://github.com/madmaze/pytesseract) and optionally 2 other providers (see extra credit).

You will evaluate it on a commercial real lease estate PDF document we’ll provide. The first step is to get the ground truth (ie. correct) text from the PDF. For this we suggest using the Anthropic API with vision mode:

- Prompt it with something simple like “Output all the text in this page”
- Make one API request per page and concatenate the text results.

Then run OCR using Tesseract or the other providers listed below and evaluate against the ground truth on accuracy. Also report speed.

Notes:

1. To evaluate accuracy you will likely need to do compute some kind of fuzzy string-match score. It’s up to you to decide what metric you want to use and explain why.
2. The reason we want to use Tesseract or an alternate OCR provider rather than an LLM is because we need bounding boxes (x,y coordinates) and LLMs don’t provide that.

Extra credit:

- Also implement and evaluate [MathPix](https://mathpix.com/ocr) and/or [Google Cloud Vision](https://cloud.google.com/vision/docs/ocr)

## Lease Document

/bagel_jays.pdf

## Deliverables

- A python (Jupyter) notebook with your implementation and evaluation code.
- An explanation of your work in English either in the notebook or in a separate readme file.

## How much time you should spend

- You can spend as little as 2-3 hours or as much time as you’d like.
- Also report how much time you spent on the task, as your work will be evaluated based on the amount of time spent.
- Beyond just the basic implementation/evaluation task there are several ways to get more detailed - it’s up to you to come up with ideas for how.

## What you will be evaluated on

- Correctness - is your code correct, free of bugs, and is the conclusion of your evaluation correct?
- Code quality - is your code easy to read and maintain?
- Speed - how quickly do you work?
- Thoughtfulness - did you think through edge cases and gotchas?
- Communication - do you communicate your findings clearly and concisely?
