---
title: Getting Gemini to write an ORM for Spanner in a weekend
date: 2025-05-26
categories:
  - Experiments
tags:
  - experiments
  - spanner
  - orm
  - ai
  - gemini
  - nodejs
  - bun
---

Recently, I wanted to get a [Bun](https://bun.sh/) application working with Google Spanner. While Google provides a client library for Spanner ([`@google-cloud/spanner`](https://www.npmjs.com/package/@google-cloud/spanner)), the JavaScript (Node.js/Bun) ecosystem lacks a dedicated Object-Relational Mapper (ORM) specifically designed for Spanner's native Google SQL dialect. This gap prompted an experiment: could Google's Gemini assist in rapidly developing one? This post details that journey and introduces [spanner-orm](https://github.com/Flux159/spanner-orm/), a Google SQL ORM in Bun largely created with AI help over a weekend.

## The Spanner Conundrum in the Javascript Ecosystem

Object-Relational Mappers (ORMs) are staples in modern development, abstracting away the intricacies of database interaction. Think [ActiveRecord](https://guides.rubyonrails.org/active_record_basics.html) in Rails, [Hibernate](https://hibernate.org/orm/) in Java, [SQLAlchemy](https://www.sqlalchemy.org/) for Python, or [Prisma](https://www.prisma.io/)/[Drizzle](https://orm.drizzle.team/) in the JavaScript world. They generally make life easier even if you have to fallback to SQL every now and then.

My journey started when I decided to migrate an application from Postgres to Google Spanner. I was drawn to Spanner's horizontal scalability and some of its more advanced features like graph queries. I initially pinned my hopes on Drizzle ORM, which has excellent Postgres support, thinking I could leverage Spanner's own Postgres dialect via its `pgadapter`.

That's where the optimism met reality. I was completely wrong.

Spanner's Postgres dialect, while a clever piece of engineering (it's a Java server translating the Postgres wire protocol to Spanner-compatible queries), isn't a full-fat Postgres. I quickly ran into limitations:
1.  **Incomplete Feature Set:** Even basic, commonly used functions like `ILIKE` (case-insensitive LIKE) were unsupported. This was just the tip of the iceberg, with numerous other incompatibilities like migrations failing as I tried to integrate Drizzle.
2.  **Dialect Lock-in:** A more significant issue is that Spanner requires you to choose your database's dialect (Google SQL or Postgres) *at creation time*. Once set, you're committed without a potentially complex migration. This made the Postgres dialect feel like a risky bet if its limitations became too restrictive.

With these limitations, the path forward with existing tools looked increasingly painful.

## `spanner-orm`: An ORM written by AI

Faced with these challenges, the traditional route would be to either find a different database, compromise on features, or embark on the non-trivial task of writing a custom ORM. Writing an ORM from scratch, including thorough testing, can easily consume weeks, if not more, before it's robust enough for production.

This time, I had a different idea. What if I could leverage a Large Language Model? Gemini, Google's multimodal AI, knows Spanner and Google SQL quite well. So, using [Cline](https://cline.bot/), I set out to guide Gemini in crafting an ORM specifically for Spanner's native Google SQL dialect.

The results were astounding. Over the course of about three days, Gemini 2.5 Pro, with iterative prompting and refinement, generated the foundational framework for what I'm now calling `spanner-orm`. This wasn't just boilerplate; it was a significant chunk of the core logic.

The next three days or so were spent on rigorous testing, integrating `spanner-orm` into an existing application I had, and ironing out the kinks to ensure its features aligned with real-world usage.

One of the most impressive aspects? Gemini didn't just write code; it wrote tests. **Over 250 of them.** That's a level of diligence that many human developers, especially under a tight "weekend project" timeline, might (understandably) skimp on. This built-in test coverage was invaluable for quickly iterating and gaining confidence in the generated code.

## Introducing `spanner-orm`

And so, `spanner-orm` was born – a lightweight, modern ORM for Node.js and Bun, designed to work directly with Google Spanner using its native Google SQL dialect. It aims to provide a developer-friendly API for common database operations, type safety, and a solid foundation for building applications on Spanner without wrestling with dialect incompatibilities.

In addition, `spanner-orm` also supports postgres as a dialect, so I can use postgres or pglite locally and with the same client code target Spanner in production, making local changes significantly easier to test and also keeping the developer experience for other developers unchanged.

While it's still early days, the core is functional, and the development process itself was seamless.

## Reflections on AI-Powered Development

This experiment wasn't just about getting an ORM built; it was a glimpse into how AI can augment the development process, especially for specialized tools or bridging gaps in existing ecosystems. As a software developer myself, the idea of rapidly prototyping or even generating significant portions of foundational libraries like an ORM in a matter of days, rather than weeks or months, is a game-changer.

The collaboration with Gemini (via Cline) felt like pair programming with an incredibly knowledgeable, albeit sometimes needing precise direction, partner. It handled much of the heavy lifting and boilerplate, allowing me to focus on the architecture, specific requirements, and testing.

Could I have written this ORM myself? Yes, eventually. Could I have done it, complete with extensive tests, in a weekend while juggling other things? Absolutely not.

This experience has certainly shifted my perspective on how LLMs can be integrated into the software development lifecycle, not just for generating snippets, but for tackling substantial engineering tasks.

In addition to making the [spanner-orm repo public](https://github.com/Flux159/spanner-orm/), I also added a few notes in the notes/ folder that explain what TaskPrefix I used to guide the LLM, the phases that Gemini laid out on iteratively tackling the project, and some of the bugfix prompts I used to get Gemini to fix the code it had written.

One of my next steps is going to be trying a similar process running 3 projects at once - can I get Gemini working in a productive loop while I'm juggling testing & iterating on prompts for 3 different web apps.
