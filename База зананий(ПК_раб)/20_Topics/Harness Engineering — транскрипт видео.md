# Harness Engineering — транскрипт видео

Источник: [Edward Donner — Master AI Harness Engineering in 14 Minutes](https://youtu.be/t1PXvg2FceU)

Язык оригинала: английский. Ниже — очищенные автоматические субтитры YouTube с таймкодами. Возможны ошибки распознавания имён и терминов (например, Kimi, Qwen, Kanban, Claude Code, Vercel).

Связанные заметки: [[Harness Engineering — универсальный чек-лист разработки приложений]], [[Универсальный чек-лист среды проекта с AI-агентом]].

## Навигация

- 00:00 — демонстрация CRM и стоимость запуска
- 01:50 — определение Harness Engineering
- 02:09 — что такое harness для AI-агента
- 02:47 — prompt, context и harness engineering
- 03:31 — компоненты harness
- 05:38 — самоулучшение
- 06:23 — пример проекта CRM
- 06:43 — workflow
- 07:26 — Qwen, Kimi и субагенты
- 08:17 — песочница в dev container
- 08:34 — самоулучшение агента
- 09:24 — рекурсивное самоулучшение
- 10:12 — запуск в OpenCode
- 10:51 — результат и оценка
- 11:15 — улучшение рабочего процесса
- 12:01 — мета-улучшение процесса
- 13:11 — выводы

## Смысловой транскрипт на русском

**00:00–01:50 — результат эксперимента.** Автор показывает CRM, которую агентный harness собрал примерно за 1–1,5 часа с помощью Qwen 3.8 Max и Kimi K3. Расход на модели составил $8,71. В приложении есть дашборд, прогноз выручки, стадии воронки, организации, контакты, сделки, задачи и Kanban с drag-and-drop. По оценке автора, это лучший из его подобных CRM-экспериментов и при этом заметно дешевле предыдущего.

**01:50–02:47 — что такое harness.** AI-агент определяется как LLM, снабжённая инструментами и работающая в цикле до достижения цели. Harness — это окружающий модель каркас: он выдаёт инструменты, организует цикл, хранит ориентацию на цель и позволяет решать более сложные задачи.

**02:47–03:31 — эволюция подхода.** Prompt engineering отвечает за формулировку запроса. Context engineering — за сбор правильного входного контекста. Harness engineering включает контекст, но идёт дальше: проектирует весь каркас вокруг агента, чтобы повысить вероятность хорошего результата на сложной задаче.

**03:31–05:38 — основные компоненты.** В harness входят промпты, инструменты и память; рабочий процесс или цикл; устойчивое состояние; субагенты и команды агентов; разрешения и песочница; наблюдаемость; количественные evals; обратная связь; самоулучшение. Субагенты мощны, но требуют ясного разделения ролей. Evals позволяют модели измерить результат и целенаправленно его улучшить.

**05:38–06:43 — самоулучшение.** Harness может не только решать задачу, но и обновлять собственный процесс на основе трудностей и обратной связи. Некоторые агентные продукты дают часть механики из коробки, однако организация состояния, sandbox и улучшения процесса часто остаётся задачей разработчика — это и есть практическая Harness Engineering.

**06:43–08:34 — устройство CRM-harness.** Процесс зафиксирован в файле: собрать проект по требованиям, проверить критерии успеха, вызвать субагента для продуктового review, внести исправления и выполнить шаг самоулучшения. Основной агент на Qwen строит продукт. Reviewer на Kimi тестирует и даёт обратную связь, но не имеет права менять файлы. Проект запускается в изолированном VS Code dev container; агент получает браузерный навык для проверки собственного UI.

**08:34–10:12 — рекурсивное самоулучшение.** После сборки агент анализирует сложности и feedback, затем обновляет workflow, чтобы следующий похожий проект выполнить лучше. Дополнительный шаг просит его улучшить сам метод самоулучшения: «стать лучше в том, чтобы становиться лучше». Это автор называет recursive self-improvement.

**10:12–11:15 — запуск.** Автор запускает основной агент в OpenCode и даёт короткую команду построить проект и улучшить себя. Работа занимает около 1–1,5 часа. Агент проходит фазы сборки, review, включения обратной связи, валидации и самоулучшения.

**11:15–12:01 — обновлённый workflow.** Исходный процесс разрастается в практический playbook: сначала внимательно прочитать документацию и файл улучшений; использовать проверенный архитектурный рецепт; выполнять verification loop; помнить, что unit-тесты необходимы, но недостаточны; учитывать review и feedback.

**12:01–13:11 — мета-уроки.** Агент добавляет правила эффективного самоулучшения: классифицировать каждый урок; записывать способ его обнаружения; превращать выводы в исполнимые инструкции; удалять слабые и устаревшие правила; отличать устойчивые знания от проектных; перечитывать обновлённый процесс и замыкать feedback loop. Один конкретный урок: прежде чем считать UI сломанным, воспроизвести проблему реальными пользовательскими событиями.

**13:11–13:55 — вывод.** Рекурсивное самоулучшение — лишь один элемент системы. Сочетание процесса, состояния, ролей, sandbox, evals, обратной связи и ограниченного улучшения harness дало автору лучший результат среди повторных CRM-экспериментов.

## Полный транскрипт
**[00:00]** Everyone is talking about harness

**[00:02]** engineering. Have you wondered what that

**[00:03]** is? I'm going to explain everything.

**[00:06]** Harness engineering is in fact a whole

**[00:08]** bunch of things and one of them is a bit

**[00:10]** spooky and I'm going to take you through

**[00:11]** it all in just a second. But first I

**[00:13]** want to show you the results of running

**[00:15]** a project in an agent harness which used

**[00:18]** Kimmy K3 and also used the new Quen 3.8

**[00:23]** Max. This shows the usage of the two

**[00:25]** models. a total spend of $8.71.

**[00:29]** And let me show you what it did. For

**[00:31]** this project, I brought back one of my

**[00:33]** favorites. It is the CRM platform,

**[00:36]** asking a coding agent to build a sales

**[00:39]** CRM, like a salesforce or a pipe drive,

**[00:43]** build the whole thing from scratch for

**[00:44]** me to use a personal CRM. And this is

**[00:48]** what it came up with. And I have to tell

**[00:49]** you that this is, I think, the best one

**[00:52]** I've had so far. You're seeing here a

**[00:54]** really nice dashboard that you land on

**[00:55]** when you first come to the product. It's

**[00:57]** got expected revenue as well as full

**[01:00]** revenue based on the pipeline. It's got

**[01:03]** similar charts if you've seen the ones I

**[01:04]** did before like with GLM and also with

**[01:07]** Fable. You'll also see there's this

**[01:08]** pipeline by stage which shows new,

**[01:11]** qualified, proposed, negotiated, won,

**[01:13]** and lost. And it's showing both the

**[01:15]** total value and the expected revenue

**[01:18]** which is probability weighted. So, it's

**[01:20]** some really interesting functionality

**[01:21]** that you might see in higherend sales

**[01:24]** platforms. And then, as before, the

**[01:26]** to-do lists. It's a nice product. And we

**[01:29]** can click around and see things like the

**[01:30]** organizations as usual, all the

**[01:32]** functionality you'd expect, the

**[01:34]** contacts. There's the buttons to delete

**[01:36]** and edit the deals with all the details

**[01:39]** of different deals. And then, of course,

**[01:40]** the pipeline, which is where we have a

**[01:42]** camb board that also supports us being

**[01:45]** able to drag and drop, and the totals

**[01:47]** change. Everything is working really

**[01:49]** nicely. It's a great product. It was

**[01:51]** built in about the same time that it

**[01:53]** took to build it with Fable, but at a

**[01:55]** fraction of the price. Okay, so what is

**[01:58]** harness engineering? What does it have

**[01:59]** to do with the project I just showed

**[02:01]** you? And what's the big deal? What's

**[02:03]** this spooky thing I mentioned before?

**[02:05]** Let me now reveal all. But first, don't

**[02:08]** hate me if I give you the definition of

**[02:10]** an AI agent one more time. An agent is

**[02:14]** an LLM that's been equipped with tools

**[02:16]** and it's running in a loop in order to

**[02:19]** achieve a goal. That's the definition of

**[02:21]** an agent. And another way you sometimes

**[02:22]** hear this explained is that an agent is

**[02:24]** an LLM that's in a harness. A harness is

**[02:29]** the thing that equips it with tools,

**[02:30]** puts it in a loop, and is oriented

**[02:32]** around achieving a goal, repeatedly

**[02:35]** calling it until it decides that the

**[02:37]** goal is met. So you can think of the

**[02:38]** harness as the scaffolding built around

**[02:40]** the LLM to allow it to achieve more

**[02:43]** complex goals. So we've all been on this

**[02:45]** journey for the last few years. A couple

**[02:46]** of years ago, the big thing was prompt

**[02:48]** engineering. People were paid huge

**[02:50]** amounts of money to be prompt engineers.

**[02:53]** And then last year, this new term came

**[02:55]** along, context engineering, which was

**[02:57]** the practice of assembling the perfect

**[03:00]** input context to send to an agent so

**[03:03]** they be best positioned for successful

**[03:04]** outcomes. That was context engineering.

**[03:07]** But now context engineering is old hat.

**[03:09]** We've moved on from there. It's now all

**[03:12]** about harness engineering. Harness

**[03:14]** engineering, which is basically context

**[03:16]** engineering and a whole lot more. It's

**[03:18]** about putting together all of the

**[03:19]** scaffolding, all of the harness that

**[03:22]** goes around your LLM, around your agent

**[03:25]** to allow it to take on the hardest

**[03:27]** possible tasks and have the best

**[03:29]** possible outcomes. So, I'm going to

**[03:30]** explain what harness engineering is all

**[03:32]** about and then we're going to look at

**[03:34]** doing some harness engineering that's

**[03:36]** going to allow us to achieve the best

**[03:39]** sales CRM so far. So, what what's in

**[03:41]** harness engineering? So, obviously the

**[03:43]** first part of harness engineering of

**[03:45]** getting the right scaffolding is doing

**[03:47]** everything we're already doing around

**[03:49]** context engineering. Thinking about the

**[03:51]** prompts, the tools, the memory that

**[03:54]** we're equipping an LLM with in order to

**[03:56]** achieve a goal. That's a big part of it.

**[03:58]** And next up is the workflow or the loop.

**[04:01]** It may be just as simple as going

**[04:03]** through a to-do list or there might be

**[04:04]** something more sophisticated about how

**[04:06]** you want to work at a particular

**[04:08]** problem. And we'll get concrete on that

**[04:10]** in a second. Another point is the state.

**[04:13]** And often with coding agents

**[04:14]** particularly, state is managed using

**[04:17]** markdown files by recording information

**[04:20]** in markdown files that allows you to

**[04:22]** track state in a persistent way. But

**[04:24]** there are other ways as well. And then

**[04:26]** sub agents, something that I've used

**[04:28]** before which is can be so powerful. It

**[04:30]** can also be complicated if not used

**[04:32]** well. And then agent teams clawed agent

**[04:35]** teams is more collaborative. Another way

**[04:38]** of breaking down your tasks into

**[04:40]** different agent loops that run

**[04:42]** independently. That's also part of

**[04:44]** harness engineering. And then having the

**[04:46]** right permissions, potentially running

**[04:48]** in a sandbox to control what an agent is

**[04:51]** able to do. And then observability,

**[04:53]** being able to watch what's going on and

**[04:56]** being able to evaluate it. You know how

**[04:58]** much I go on about evaluations. So

**[05:00]** important. That is also part of harness

**[05:02]** engineering. Giving your model the

**[05:04]** ability to quantitively evaluate its own

**[05:07]** results so that it can improve on them.

**[05:09]** Such an an easy way to get better

**[05:12]** outcomes from coding agents as we saw in

**[05:14]** my last video on the Groove Box. If you

**[05:16]** haven't seen that, that is observability

**[05:19]** and evaluations. And then last but not

**[05:21]** least, self-improvement.

**[05:23]** Self-improvement is about building

**[05:25]** features into your agent harness so that

**[05:28]** it's able to evolve itself. It's able to

**[05:30]** change the agent harness and get better

**[05:33]** and better at solving harder problems.

**[05:35]** That's self-improvement. Now, there's

**[05:37]** one part of self-improvement, a

**[05:39]** particular technique within

**[05:41]** self-improvement that's pretty spooky,

**[05:44]** and I'm not going to tell you right now.

**[05:45]** I'm going to show it to you later. And I

**[05:48]** think you'll agree that's something

**[05:49]** that's that's quite an eyeopener. We

**[05:51]** will get on to that. Now, some of these

**[05:54]** just come out of the box with agent

**[05:56]** products that you might be using. So

**[05:58]** like if you use Hermes, then it

**[06:00]** basically has all of these. It's like

**[06:01]** the Hermes feature list right here as we

**[06:04]** discovered when we did it in a previous

**[06:05]** video. If you're using something like

**[06:07]** clawed code or open code, then you get

**[06:10]** some of this list, but some of it is up

**[06:12]** to you. So, how you handle state in

**[06:15]** markdown files or how you handle

**[06:17]** sandboxing, do you set that up? Do you

**[06:19]** have self-improvement built into your

**[06:21]** process? That's really up to you. And

**[06:23]** configuring that, well, that's an

**[06:25]** activity. And the activity is called

**[06:27]** harness engineering. And now I have set

**[06:30]** up a simple project to build a CRM and

**[06:33]** I've incorporated many aspects of

**[06:35]** harness engineering to show you

**[06:37]** firsthand. And even if you don't

**[06:39]** particularly want to dive into code, you

**[06:40]** don't need to. It's going to be crystal

**[06:42]** clear everything I'm doing and how it

**[06:44]** sets up our agent harness. Let me show

**[06:47]** you. The first on my list after context

**[06:49]** engineering was workflow. And let me

**[06:52]** show you here. There's a file called

**[06:54]** process.mmd

**[06:55]** and it gives a build process basically a

**[06:58]** workflow that we're asking for in order

**[07:01]** to build this project. Build the entire

**[07:03]** project as documented in the

**[07:05]** requirements. Ensure success criteria

**[07:07]** are met. Use a sub agent to check the

**[07:10]** final product, incorporate any changes,

**[07:12]** and then follow the instructions in

**[07:14]** self-improve to improve yourself. We

**[07:17]** will come back to that, but this shows

**[07:20]** the workflow laid out. Now, I'm going to

**[07:22]** be using open code today. And for open

**[07:25]** code, there's a JSON file called open

**[07:27]** code.json that describes the different

**[07:30]** sub aents. And this is how I've set up

**[07:32]** my sub agent team. I have a primary

**[07:35]** agent that is instructed to follow the

**[07:37]** process in process.mmd that we were just

**[07:39]** looking at to build the project and

**[07:41]** improve yourself and is going to be

**[07:43]** using Quen 3.8 8 Max and then we have a

**[07:46]** product review sub agent that tries out

**[07:50]** the product and provides feedback and

**[07:52]** we'll be using Kimmy K3 for this and

**[07:55]** they've been set up with the appropriate

**[07:57]** permissions so that for example the

**[07:59]** reviewer is not able to change any files

**[08:01]** it just has to take care of its review

**[08:04]** and obviously it's also through this

**[08:05]** review that I'm able to have a kind of

**[08:07]** evaluation of what's happening and make

**[08:09]** sure there's a feedback loop to get

**[08:11]** better and then when it comes to sandbox

**[08:14]** boxing. I'm using dev container, the

**[08:16]** functionality built into VS Code so that

**[08:19]** it can run in an isolated Docker

**[08:21]** container. And I've got the instructions

**[08:23]** here that sets it up and configures it

**[08:25]** so that it installs open code and it

**[08:27]** also installs a skill in the form of the

**[08:29]** agent browser skill from Versel so that

**[08:32]** it's able to test its own work. And then

**[08:34]** last but not least, the selfimprove. So

**[08:37]** this is where I've added to my workflow

**[08:40]** this idea that the harness needs to be

**[08:42]** able to improve itself. So when I open

**[08:44]** this, it says you must complete this

**[08:46]** step, look back at your work in building

**[08:48]** the project, including any challenges

**[08:50]** you encountered and the feedback and

**[08:52]** then update your own process, update its

**[08:56]** workflow to factor in the learnings so

**[08:59]** that it does better next time. This is

**[09:02]** self-improvement in action. When this

**[09:04]** runs, not only will it build a CRM, but

**[09:07]** it's also going to update its own

**[09:09]** process, so it'll be able to build

**[09:11]** similar things in the future better.

**[09:13]** Okay. And now for the spooky one. Now

**[09:16]** for the weird one. Look at step four.

**[09:19]** This is what's known as recursive

**[09:21]** self-improvement. And some people don't

**[09:23]** fully appreciate what this means, but

**[09:25]** it's explained right here. Update this

**[09:28]** very file self-improve.md

**[09:31]** so that you are more effective at

**[09:33]** improving yourself in the future.

**[09:36]** Improve the way you improve yourself.

**[09:39]** Get better at getting better. That's the

**[09:42]** idea of recursive self-improvement. We

**[09:44]** want it not only to improve its own

**[09:46]** process, but to improve the way that it

**[09:48]** improves its process. That's crazy. And

**[09:51]** so all of these activities, setting up

**[09:53]** sub agents, putting in place a sandbox,

**[09:56]** having a process, having a feedback

**[09:58]** evaluation loop with a sub agent, and

**[10:01]** then having this improvement, this

**[10:02]** self-improvement and even recursive

**[10:04]** self-improvement step. This is all what

**[10:07]** goes into harness engineering. That's

**[10:09]** what I've done. And now it's time to put

**[10:12]** it into action. And so now here I am

**[10:14]** inside a dev container and I'm going to

**[10:16]** type open code to launch open code. Here

**[10:20]** it is. And now I'm going to press shift

**[10:22]** tab to flip over to my build and improve

**[10:26]** primary agent. And we're ready to go.

**[10:28]** And I'm just going to say to it like, go

**[10:30]** ahead, build a project. Improve

**[10:31]** yourself. Do do your business. Let's see

**[10:34]** what it does. It's using Quen 3.8 Max.

**[10:37]** It's looking around at what files it can

**[10:40]** access. And it's going to go off and do

**[10:42]** its thing. And I will see you in a

**[10:45]** minute. All right. So, it's not exactly

**[10:46]** been a minute. It's been like an hour,

**[10:48]** an hour and a half. I've gone and had

**[10:50]** lunch. But here are the results. You can

**[10:53]** see that it's gone through and checked

**[10:55]** off everything that's to-do list, the

**[10:57]** phases, the product review sub agent,

**[11:00]** and then the self-improvement phase is

**[11:02]** checked off. And you can see it's given

**[11:04]** the results of the personal CRM build,

**[11:07]** the validation where it evaluated

**[11:09]** itself, the feedback that got

**[11:10]** incorporated, and then finally the

**[11:13]** self-improvement. And the satisfying

**[11:15]** thing about this is that you can see how

**[11:17]** the efforts that we put in to harness

**[11:19]** engineering have paid off. We can see

**[11:21]** how they've been reflected in what it

**[11:23]** actually did. We had started out with a

**[11:25]** simple five-step process. This is what

**[11:27]** we have now in process.mmd. It's

**[11:29]** significantly built out. The process now

**[11:32]** has an extra item at the beginning about

**[11:34]** reading all the documentation carefully

**[11:36]** at the beginning including the

**[11:37]** improvement file. And then we've got

**[11:39]** like a playbook with learnings from

**[11:42]** previous runs. architecture recipe that

**[11:44]** worked, verification loop, unit tests

**[11:47]** are necessary, but not sufficient, and

**[11:49]** review and feedback. So, there's lots of

**[11:51]** copious notes it's written for itself to

**[11:53]** guide itself so that it has an improved

**[11:56]** process next time based on what it

**[11:58]** learned. But, I'm interested to see if

**[12:00]** it's actually been able to update the

**[12:03]** way that it improves, if it's done some

**[12:05]** recursive self-improvement in a

**[12:07]** meaningful, useful way. So, let's look

**[12:09]** at this file and see what it's done. So

**[12:12]** this part here looks similar to what we

**[12:13]** had before, but it has added a new

**[12:16]** section. How to self-improve

**[12:18]** effectively. Meta learnings. Do the

**[12:21]** reflection in this structured way. All

**[12:23]** right, let's see what it came up with.

**[12:24]** It says categorize every learning before

**[12:26]** writing it down. That seems very

**[12:27]** reasonable. It's got record the

**[12:29]** detection method. Write them as

**[12:31]** executable instructions. Interesting.

**[12:33]** Prune. Distinguish durable from project

**[12:36]** improvements. Close the loop by reading

**[12:38]** it once you've written. That's great.

**[12:40]** and then a a specific tool quirk. Before

**[12:44]** you're sure that the app is broken,

**[12:46]** reproduce it with real user events.

**[12:48]** Fascinating. These seem like they are

**[12:50]** legit self-improvement learnings. Now,

**[12:53]** obviously, I don't want to overdwell on

**[12:55]** recursive self-improvement. It's a nice

**[12:57]** feature, but it's just one of many

**[12:59]** aspects of harness engineering that we

**[13:01]** put into practice, and it allowed us to

**[13:03]** get what I would say is the best CRM

**[13:06]** that I've built so far in many times of

**[13:09]** doing this. So, I think we've done well

**[13:10]** and I think it's a direct consequence of

**[13:13]** the effort we put in as part of harness

**[13:15]** engineering and you should try doing

**[13:17]** some harness engineering yourself.

**[13:19]** There'll be instructions in the

**[13:20]** description below to bring up this repo.

**[13:22]** Play around with it. Get it to build you

**[13:24]** a CRM and I hope it's as gorgeous as my

**[13:27]** one right here. This is really nice. And

**[13:29]** you can also just use this one if you'd

**[13:30]** like a nice personal CRM. You'll find

**[13:32]** details below as well. It's really

**[13:34]** great. Check this out. If I come here

**[13:36]** and I move something, I get a little

**[13:38]** toast down here telling me that it's

**[13:40]** moved. And then two toasts like that.

**[13:42]** It's just great. There's so much

**[13:43]** functionality here. So, I hope you like

**[13:45]** the CRM and I hope you enjoyed this

**[13:47]** video. And if so, please do like and

**[13:49]** subscribe. That's how I know you're

**[13:50]** really there and you want me to make

**[13:52]** more of these and I will. And I hope to

**[13:53]** see you very soon for another
