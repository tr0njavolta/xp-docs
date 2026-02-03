---
title: "Welcome to Crossplane"
weight: -1
description: "Build control planes without needing to write code"
cascade:
    version: "2.1"
---

{{< whats-new-banner >}}

<div style="display: flex; align-items: center; gap: 30px; max-width: 900px; margin: 2rem 0;">
  <div style="display: flex; align-items: center; gap: 15px;">
    <div style="font-size: 3rem;">🚀</div>
    <div style="text-align: left;">
      <h1 style="margin: 0;">Crossplane</h1>
      <p style="margin: 0; opacity: 0.8;">The cloud native control plane framework</p>
    </div>
  </div>
</div>

## What is Crossplane?

Crossplane connects your Kubernetes cluster to external, non-Kubernetes resources, and allows platform teams to build custom Kubernetes APIs to consume those resources.


**Multi-cloud native:** Connect to AWS, Azure, GCP, and 80+ providers from a single control plane</p>

**Gitops ready:** Manage infrastructure as code with kubectl and your favorite GitOps tools</p>

**Custom APIs:** Build platform APIs tailored to your organization's needs</p>


Crossplane creates Kubernetes resources that represent external infrastructure. These resources can be anything from cloud provider services to on-premises hardware.

## Try Crossplane

Choose your learning path and start building with Crossplane.

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; margin: 2rem 0;">

<a href="https://killercoda.com/crossplane" style="padding: 2rem; border-radius: 12px; background: linear-gradient(135deg, rgba(0, 200, 200, 0.05) 0%, rgba(0, 200, 200, 0.08) 100%); border: 2px solid var(--border-color); text-decoration: none; color: var(--body-font-color); display: block; transition: all 0.3s ease; position: relative; overflow: hidden;" onmouseover="this.style.borderColor='var(--content-link-color)'; this.style.boxShadow='0 8px 24px rgba(0,0,0,0.12)';" onmouseout="this.style.borderColor='var(--border-color)'; this.style.boxShadow='none';">
  <div style="position: absolute; top: 0; right: 0; width: 80px; height: 80px; background: linear-gradient(135deg, var(--aqua-400), var(--aqua-400)); opacity: 0.1; border-radius: 0 12px 0 100px;"></div>
  <div style="position: relative; z-index: 1;">
    <div style="font-size: 2.5rem; margin-bottom: 0.75rem;">🎯</div>
    <h3 style="margin: 0 0 0.75rem 0; font-size: 1.25rem; font-weight: 600;">Composition Guide</h3>
    <p style="margin: 0 0 1.25rem 0; opacity: 0.8; font-size: 0.95rem;">Build custom APIs by composing resources. Try it in your browser or follow the guide.</p>
    <div style="display: flex; align-items: center; gap: 0.5rem; color: var(--content-link-color); font-weight: 600; font-size: 0.9rem;">
      Start Learning
      <span style="font-size: 1rem;">→</span>
    </div>
  </div>
</a>

<a href="{{<ref "get-started/get-started-with-managed-resources">}}" style="padding: 2rem; border-radius: 12px; background: linear-gradient(135deg, rgba(100, 200, 100, 0.05) 0%, rgba(100, 200, 100, 0.08) 100%); border: 2px solid var(--border-color); text-decoration: none; color: var(--body-font-color); display: block; transition: all 0.3s ease; position: relative; overflow: hidden;" onmouseover="this.style.borderColor='var(--content-link-color)'; this.style.boxShadow='0 8px 24px rgba(0,0,0,0.12)';" onmouseout="this.style.borderColor='var(--border-color)'; this.style.boxShadow='none';">
  <div style="position: absolute; top: 0; right: 0; width: 80px; height: 80px; background: linear-gradient(135deg, var(--grass-400), var(--grass-400)); opacity: 0.1; border-radius: 0 12px 0 100px;"></div>
  <div style="position: relative; z-index: 1;">
    <div style="font-size: 2.5rem; margin-bottom: 0.75rem;">☁️</div>
    <h3 style="margin: 0 0 0.75rem 0; font-size: 1.25rem; font-weight: 600;">Managed Resources</h3>
    <p style="margin: 0 0 1.25rem 0; opacity: 0.8; font-size: 0.95rem;">Manage cloud infrastructure with kubectl. Control AWS, Azure, GCP and more.</p>
    <div style="display: flex; align-items: center; gap: 0.5rem; color: var(--content-link-color); font-weight: 600; font-size: 0.9rem;">
      Get Started
      <span style="font-size: 1rem;">→</span>
    </div>
  </div>
</a>

<a href="{{<ref "get-started/get-started-with-operations">}}" style="padding: 2rem; border-radius: 12px; background: linear-gradient(135deg, rgba(200, 150, 50, 0.05) 0%, rgba(200, 150, 50, 0.08) 100%); border: 2px solid var(--border-color); text-decoration: none; color: var(--body-font-color); display: block; transition: all 0.3s ease; position: relative; overflow: hidden;" onmouseover="this.style.borderColor='var(--content-link-color)'; this.style.boxShadow='0 8px 24px rgba(0,0,0,0.12)';" onmouseout="this.style.borderColor='var(--border-color)'; this.style.boxShadow='none';">
  <div style="position: absolute; top: 0; right: 0; width: 80px; height: 80px; background: linear-gradient(135deg, var(--sun-400), var(--sun-400)); opacity: 0.1; border-radius: 0 12px 0 100px;"></div>
  <div style="position: relative; z-index: 1;">
    <div style="font-size: 2.5rem; margin-bottom: 0.75rem;">⚙️</div>
    <h3 style="margin: 0 0 0.75rem 0; font-size: 1.25rem; font-weight: 600;">Automate Operations</h3>
    <p style="margin: 0 0 1.25rem 0; opacity: 0.8; font-size: 0.95rem;">Run operational tasks and day-two workflows like monitoring, scaling, and maintenance.</p>
    <div style="display: flex; align-items: center; gap: 0.5rem; color: var(--content-link-color); font-weight: 600; font-size: 0.9rem;">
      Learn More
      <span style="font-size: 1rem;">→</span>
    </div>
  </div>
</a>

<a href="{{<ref "get-started">}}" style="padding: 2rem; border-radius: 12px; background: linear-gradient(135deg, rgba(150, 100, 200, 0.05) 0%, rgba(150, 100, 200, 0.08) 100%); border: 2px solid var(--border-color); text-decoration: none; color: var(--body-font-color); display: block; transition: all 0.3s ease; position: relative; overflow: hidden;" onmouseover="this.style.borderColor='var(--content-link-color)'; this.style.boxShadow='0 8px 24px rgba(0,0,0,0.12)';" onmouseout="this.style.borderColor='var(--border-color)'; this.style.boxShadow='none';">
  <div style="position: absolute; top: 0; right: 0; width: 80px; height: 80px; background: linear-gradient(135deg, rgba(150, 100, 200, 0.3), rgba(150, 100, 200, 0.3)); opacity: 0.1; border-radius: 0 12px 0 100px;"></div>
  <div style="position: relative; z-index: 1;">
    <div style="font-size: 2.5rem; margin-bottom: 0.75rem;">💻</div>
    <h3 style="margin: 0 0 0.75rem 0; font-size: 1.25rem; font-weight: 600;">Local Setup</h3>
    <p style="margin: 0 0 1.25rem 0; opacity: 0.8; font-size: 0.95rem;">Install Crossplane locally and build your first control plane with step-by-step guides.</p>
    <div style="display: flex; align-items: center; gap: 0.5rem; color: var(--content-link-color); font-weight: 600; font-size: 0.9rem;">
      Install Now
      <span style="font-size: 1rem;">→</span>
    </div>
  </div>
</a>

</div>

**Not sure which path?** Check [Foundations]({{<ref "foundations">}}) to understand core concepts first.

## Why Crossplane?

Infrastructure is complicated. As a Platform Engineer or DevOps Architect, your responsibility is to create a stable platform to serve your organization. Your developers need cloud resources and you need to manage them.

You're interested in Crossplane because infrastructure-as-code alone isn't enough. You're building a control plane for your organization—a sophisticated, self-service-minded architecture framework.

You need Crossplane because building custom infrastructure APIs is the future of resource management. To make Crossplane work for you, you need to:

- Understand cloud resources
- Make architectural decisions about what parameters to expose to your users
- Build production-ready patterns your team can trust

**Key Benefits:**

- ✅ **Self-service infrastructure** - Developers provision with kubectl
- ✅ **Reduced complexity** - Hide infrastructure details behind simple APIs
- ✅ **Policy enforcement** - Centralized governance and compliance
- ✅ **Continuous reconciliation** - Ensure actual state matches desired state
- ✅ **Full GitOps support** - Infrastructure as code workflows


## Join the Community

<div style="border-radius: 0.75rem; padding: 2rem; text-align: center; margin-top: 3rem; background: var(--body-background); border: 1px solid var(--border-color);">

Get help, share your experience, and contribute to making Crossplane better.

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 1rem; margin-top: 1.5rem;">
  <a href="https://slack.crossplane.io" style="padding: 0.75rem 1.5rem; border-radius: 6px; text-decoration: none; font-weight: 600; background: transparent; color: var(--content-link-color); border: 2px solid var(--content-link-color); transition: all 0.3s ease;" onmouseover="this.style.background='var(--content-link-color)'; this.style.color='white';" onmouseout="this.style.background='transparent'; this.style.color='var(--content-link-color)';">
    💬 Join Slack
  </a>
  <a href="https://github.com/crossplane/crossplane" style="padding: 0.75rem 1.5rem; border-radius: 6px; text-decoration: none; font-weight: 600; background: var(--content-link-color); color: white; border: 2px solid var(--content-link-color); transition: all 0.3s ease;" onmouseover="this.style.opacity='0.9';" onmouseout="this.style.opacity='1';">
    ⭐ GitHub
  </a>
  <a href="https://blog.crossplane.io" style="padding: 0.75rem 1.5rem; border-radius: 6px; text-decoration: none; font-weight: 600; background: transparent; color: var(--content-link-color); border: 2px solid var(--content-link-color); transition: all 0.3s ease;" onmouseover="this.style.background='var(--content-link-color)'; this.style.color='white';" onmouseout="this.style.background='transparent'; this.style.color='var(--content-link-color)';">
    📝 Blog
  </a>
</div>

</div>
