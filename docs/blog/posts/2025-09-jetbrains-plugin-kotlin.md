---
title: "My first JetBrains plugin, in a language I don't know"
description: How I built and published LLM Brains, a JetBrains plugin in Kotlin, with the AI coding agents Codex CLI and Junie
date: 2025-09-19
authors:
  - pforret
categories:
  - A.I.
tags:
  - coding agents
  - JetBrains
  - Junie
  - Kotlin
---

# My first JetBrains plugin, in a language I don't know

![Creating my first JetBrains plugin with Junie and Kotlin](img/llm-brains-jetbrains-plugin.jpg)

> TL;DR: I created a Jetbrains plugin in Kotlin, a language I don't master, with Jetbrains' own Junie AI Agent.
> It went quite easy, and now I have my first published plugin in the Jetbrains marketplace.

<!-- more -->

## The itch

I run Claude Code inside PhpStorm, with a button that opens a Claude Code terminal.
I wanted two things on top of that: terminals that are easy to recognise by their title, and the same one-click start
for the other terminal coding agents, like Codex CLI and Gemini CLI.

The catch: JetBrains plugins are written in **Kotlin**, against the **JetBrains SDK**.
I'm at home in bash, PHP, Go and Python, but I had never touched either of these.

## Two coding agents, two outcomes

**Codex CLI** went first. It produced a lot of code and a handy Docker-based Gradle build, so I didn't have to install a Java SDK.
But the build fought obsolete methods and missing dependencies, and when I finally installed the plugin, the button showed up but did nothing.
Hard to debug when you don't really understand the code.

**Junie**, JetBrains' own coding agent, did much better. I kept the build scripts and restarted from the source.
Junie knew its way around the JetBrains SDK, produced working code, and worked through the deprecation warnings
from the Marketplace review (it took a few rounds). It also added the logo to the button and the Marketplace listing.

## The result

**LLM Brains** is a JetBrains plugin that opens any LLM coding agent in a terminal: Claude, Codex, Gemini and more.

- :fontawesome-brands-github: Source: [toolstud-io/LlmBrains](https://github.com/toolstud-io/LlmBrains)
- :material-puzzle: [LLM Brains on the JetBrains Marketplace](https://plugins.jetbrains.com/plugin/28522-llm-brains)

My first JetBrains plugin, in a language I don't know, with an SDK I don't know.
For me that's what A.I.-assisted software development is about: the agent takes care of the unfamiliar parts,
so you can build what you need.

[Read the full story on blog.forret.com](https://blog.forret.com/2025/2025-09-19/jetbrains-plugin-kotlin/){ .md-button .md-button--primary }

*Header photo: Milad Fakurian on Unsplash, created with [splashmark](https://github.com/pforret/splashmark).*
