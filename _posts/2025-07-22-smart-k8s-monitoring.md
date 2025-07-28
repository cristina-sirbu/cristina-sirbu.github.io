---
title: "Building a Smart Kubernetes Monitor"
subtitle: "Using AI to Debug Kubernetes Issues"
date: 2025-07-22 17:00
layout: post
tags: [Kubernetes, Prometheus, LLM, DevOps, AI, Monitoring, On-call]
background: '/img/posts/how-i-built-my-dev-portofolio.jpg'
---

I think many are familiar with the following scenario: being on call and getting paged in the middle of the night, opening
the alert and staring at it thinking: *okay... what now?*

This project started with that feeling.

### The idea

I wanted to build something small and practical:  
a tool that not only tells me that something is wrong in my Kubernetes cluster, but also helps me understand why and maybe
what to do next.

So I created **smart-k8s-monitor**:  
a tiny system that monitors Kubernetes workloads, detects issues (like pods restarting too often) and sends those alerts
to a local AI model. The model reads the alert and suggests a possible fix, which then gets forwarded to Discord.

You can replace Discord with Slack, Microsoft Teams or whatever you use internally.

The LLM I use is [Mistral](https://ollama.com/library/mistral), running locally via [Ollama](https://ollama.com).  
No API keys, no network traffic to OpenAI.  
I didn’t want to send sensitive data to a third-party provider. Security and privacy matter. Having your own local LLM 
gives you full control.

I chose the Mistral model mainly because of its smaller size and quick startup. It’s fast enough to run locally on a 
laptop and still gives reasonable responses. If you want something larger or more capable, Ollama supports a whole list of models:
https://ollama.com/library. For example, `llama3` (more powerful, but larger) or `codellama` (code focused).

In this project I am choosing to deploy locally just because it is a Proof-of-Concept. In a real-world enterprise environment, 
I would probably use Docker containers running in a cloud VM (AWS EC2, GCP Compute Engine) or deploy it inside a Kubernetes
cluster as a service. This way, the LLM still runs within the company's infrastructure boundaries (no data living your organization)
which is critical when dealing with internal service names, credentials or failure details.

---

### How it works

It looks like this:

crashy pod → Prometheus alert → Alertmanager → Python webhook → LLM (Mistral) → Discord

Since this is a proof-of-concept, I deployed the entire project locally. I've used:
- Minikube for the cluster
- Helm for installing Prometheus stack
- Python + Flask for the alert handler
- Ollama for local LLM inference
- Ngrok to expose the webhook to the cluster

---

### What you get

Once it’s set up, you can:
- Simulate a crashing pod
- Get alerted when that pod enters `CrashLoopBackOff`
- Have the AI respond with something like:
  > “This may be due to a missing config or an invalid image. Try running `kubectl describe pod` to get more detail.”

Sometimes that’s enough to nudge you in the right direction, especially at 3 a.m. when your brain isn’t firing at full capacity.

---

### Some lessons

I learned a lot while building this project:

* **Testing alert delivery can be tricky.** I had to create a “crashy” pod that deliberately fails so I could simulate 
`CrashLoopBackOff`. That part worked well, but verifying the alert fired, that it matched the right rule and that it 
reached my webhook took several steps. I relied a lot on Prometheus UI and logs from the alertmanager pod.

* **Alertmanager routing is fragile.** If your routing config is off (e.g. missing a receiver or misplacing a route), 
Alertmanager will silently fail to load the config and no alert will get delivered. In my case, it turned out I needed 
to define a fallback "null" receiver to satisfy the Prometheus Operator's validation rules.

* **Filtering only specific alerts requires careful config.** I wanted to only forward custom alerts (like HighPodRestarts)
to Discord. That required setting up match rules correctly and defining routes in the right order. A misplaced matcher 
would cause all alerts to go to the webhook or none at all.

* **Ollama doesn’t tell you if a model is already running.** So when I tried to run `mistral` while `llama3` was still 
loaded, I got timeouts instead of a clear error. You need to stop one model before starting another.

* **It’s worth having help at 3 a.m.** Sometimes you're too tired to troubleshoot and just having a second “brain” that 
gives you a suggestion like “Check the init container logs” can save time and frustration. That’s really what this was 
about: reducing friction in moments when you need support the most and none of your colleagues may be available or awake.

But I also realized that building this was fun. It gave me ideas for how AI can support operations work in small, 
lightweight ways. Not as a replacement, but as a second pair of eyes.

---

### Try it

All the code and instruction are here:  
👉 [https://github.com/cristina-sirbu/smart-k8s-monitor](https://github.com/cristina-sirbu/smart-k8s-monitor)

And if you're interested in combining observability with AI, feel free to fork it and adapt it to your own workflows.