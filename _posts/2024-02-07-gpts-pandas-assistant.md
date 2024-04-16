---
title: GPTs -- Pandas Assistant
date: 2024-02-07
tags:
- gpt
- pandas
---

* TOC
{:toc}

I created a "Pandas Assistant" GPTs. Here are some notes.

## Notes

### Step 1: Create

Go to <https://chat.openai.com/create>, navigate to the `Create` tab.
Then "talk" to ChatGPT about what I want.
For example, I made the following requests:

1. "Make a coding assistant who specialize pandas, the python data analysis library"
2. "Give it a name: "pandas assistant"." (I didn't like the original name ChatGPT came up with.)
3. Accept the profile photo ChatGPT created.
   ![logo](/assets/images/20240207-pandas-assistant-logo.png)
4. "It is a coding assistant. When user ask something like "How do I rename a column?" The assistant understands that they wants to rename a column in pandas dataframe, and generate some code example for the user"
5. "Renaming columns is just one example, there are many other operations in pandas that we want the assistant to help with. Such as apply a transformation to a column, join two data frames, count number of unique values in a series, etc."
6. "I want it to be able to generate code examples, but not too verbose"

### Step 2: Configure

Go to the `Configure` tab, which now has `Name`, `Description` and `Instructions` filled in.
I believe this is what ChatGPT "distilled" from the previous conversation and used in the future.
In other words, the conversation above will be discarded.

The instructions look like this:

> As a coding assistant specializing in Pandas, the Python data analysis library,
> I'm here to assist with a wide range of data manipulation, analysis, and visualization tasks. 
> My role is to provide precise code examples for operations like
> renaming columns, applying transformations, joining DataFrames, and more.
> While I aim to cover the breadth of the Pandas library, I understand the importance of conciseness.
> Therefore, I'll ensure my code examples are straightforward and to the point,
> avoiding verbosity to make your data analysis tasks simpler and more efficient.
> If a request is unclear, I'll seek clarification to deliver accurate and useful solutions,
> but I'll also try to autonomously fill in missing details when possible.
> My focus is on delivering clear, actionable, and concise solutions to help you with your Pandas-related queries.

I also downloaded the latest version (v2.2.0) of the pandas documentation from
<https://pandas.pydata.org/docs/> as a zip file and uploaded it to the "knowledge".

Finally, I checked the `Code Interpreter` option.

### Step 3: Save and Use

I saved and published the GPTs to everyone.
It can now be accessed at <https://chat.openai.com/g/g-rmsqtXhJU-pandas-assistant>,
and summoned with `@Pandas Assistant`.

## References

* <https://openai.com/blog/introducing-gpts>
* <https://www.techtarget.com/whatis/feature/Custom-GPTs-Examples-and-how-to-build>
