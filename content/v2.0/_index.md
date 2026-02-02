---
title: "Welcome to Crossplane"
weight: -1
description: "Build control planes without needing to write code"
cascade:
    version: "2.0"
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

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1.5rem; margin: 2rem 0;">

<div style="padding: 1.5rem; border-radius: 8px; background: var(--body-background); border: 1px solid var(--border-color);">
  <div style="display: flex; align-items: center; margin-bottom: 1rem;">
    <span style="font-size: 1.5rem; margin-right: 0.75rem;">☁️</span>
    <h3 style="margin: 0; font-size: 1.25rem;">Multi-Cloud Native</h3>
  </div>
  <p style="margin: 0; opacity: 0.8;">Connect to AWS, Azure, GCP, and 80+ providers from a single control plane</p>
</div>

<div style="padding: 1.5rem; border-radius: 8px; background: var(--body-background); border: 1px solid var(--border-color);">
  <div style="display: flex; align-items: center; margin-bottom: 1rem;">
    <span style="font-size: 1.5rem; margin-right: 0.75rem;">📦</span>
    <h3 style="margin: 0; font-size: 1.25rem;">GitOps Ready</h3>
  </div>
  <p style="margin: 0; opacity: 0.8;">Manage infrastructure as code with kubectl and your favorite GitOps tools</p>
</div>

<div style="padding: 1.5rem; border-radius: 8px; background: var(--body-background); border: 1px solid var(--border-color);">
  <div style="display: flex; align-items: center; margin-bottom: 1rem;">
    <span style="font-size: 1.5rem; margin-right: 0.75rem;">🎯</span>
    <h3 style="margin: 0; font-size: 1.25rem;">Custom APIs</h3>
  </div>
  <p style="margin: 0; opacity: 0.8;">Build platform APIs tailored to your organization's needs</p>
</div>

</div>

Crossplane creates Kubernetes resources that represent external infrastructure. These resources can be anything from cloud provider services to on-premises hardware.

## Try Crossplane

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; margin: 2rem 0;">

<a href="https://killercoda.com/crossplane" style="padding: 2rem; border-radius: 12px; background: linear-gradient(135deg, rgba(0, 200, 200, 0.05) 0%, rgba(100, 200, 100, 0.05) 100%); border: 2px solid var(--border-color); text-decoration: none; color: var(--body-font-color); display: block; transition: all 0.3s ease; position: relative; overflow: hidden;" onmouseover="this.style.borderColor='var(--content-link-color)'; this.style.boxShadow='0 8px 24px rgba(0,0,0,0.12)';" onmouseout="this.style.borderColor='var(--border-color)'; this.style.boxShadow='none';">
  <div style="position: absolute; top: 0; right: 0; width: 80px; height: 80px; background: linear-gradient(135deg, var(--aqua-400), var(--grass-400)); opacity: 0.1; border-radius: 0 12px 0 100px;"></div>
  <div style="position: relative; z-index: 1;">
    <div style="font-size: 2.5rem; margin-bottom: 0.75rem;">🎮</div>
    <h3 style="margin: 0 0 0.75rem 0; font-size: 1.25rem; font-weight: 600;">Interactive Playground</h3>
    <p style="margin: 0 0 1.25rem 0; opacity: 0.8; font-size: 0.95rem;">No installation required. Learn Crossplane in your browser with guided, hands-on tutorials.</p>
    <div style="display: flex; align-items: center; gap: 0.5rem; color: var(--content-link-color); font-weight: 600; font-size: 0.9rem;">
      Launch Tutorial
      <span style="font-size: 1rem;">→</span>
    </div>
  </div>
</a>

<a href="{{<ref "get-started">}}" style="padding: 2rem; border-radius: 12px; background: linear-gradient(135deg, rgba(100, 200, 100, 0.05) 0%, rgba(200, 200, 100, 0.05) 100%); border: 2px solid var(--border-color); text-decoration: none; color: var(--body-font-color); display: block; transition: all 0.3s ease; position: relative; overflow: hidden;" onmouseover="this.style.borderColor='var(--content-link-color)'; this.style.boxShadow='0 8px 24px rgba(0,0,0,0.12)';" onmouseout="this.style.borderColor='var(--border-color)'; this.style.boxShadow='none';">
  <div style="position: absolute; top: 0; right: 0; width: 80px; height: 80px; background: linear-gradient(135deg, var(--grass-400), var(--sun-400)); opacity: 0.1; border-radius: 0 12px 0 100px;"></div>
  <div style="position: relative; z-index: 1;">
    <div style="font-size: 2.5rem; margin-bottom: 0.75rem;">💻</div>
    <h3 style="margin: 0 0 0.75rem 0; font-size: 1.25rem; font-weight: 600;">Local Setup</h3>
    <p style="margin: 0 0 1.25rem 0; opacity: 0.8; font-size: 0.95rem;">Install Crossplane locally and build your first control plane with step-by-step guides.</p>
    <div style="display: flex; align-items: center; gap: 0.5rem; color: var(--content-link-color); font-weight: 600; font-size: 0.9rem;">
      Get Started
      <span style="font-size: 1rem;">→</span>
    </div>
  </div>
</a>

</div>

## Choose Your Learning Path

Crossplane has core features. **Which one should you learn first?**

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1.5rem; margin: 2rem 0;">

<div style="padding: 1.75rem; border-radius: 12px; background: linear-gradient(135deg, rgba(0, 200, 200, 0.03) 0%, rgba(0, 200, 200, 0.08) 100%); border: 2px solid rgba(0, 200, 200, 0.2); position: relative; overflow: hidden;">
  <div style="position: absolute; top: -1px; left: 0; right: 0; height: 3px; background: linear-gradient(90deg, var(--aqua-400), transparent);"></div>
  <h3 style="margin: 0 0 0.75rem 0; font-size: 1.1rem; color: var(--aqua-400);">🎯 Build Custom APIs</h3>
  <p style="margin: 0 0 1rem 0; font-size: 0.95rem; opacity: 0.8;">Create platform abstractions by composing multiple resources into custom Kubernetes APIs.</p>
  <strong>Start with:</strong> <a href="{{<ref "get-started/get-started-with-composition">}}" style="color: var(--content-link-color); font-weight: 600; text-decoration: none;">Composition Guide →</a>
  <p style="margin: 0.75rem 0 0 0; font-size: 0.85rem; opacity: 0.7; border-top: 1px solid rgba(0, 200, 200, 0.1); padding-top: 0.75rem;">Best for: Platform teams, IDPs, standardization</p>
</div>

<div style="padding: 1.75rem; border-radius: 12px; background: linear-gradient(135deg, rgba(100, 200, 100, 0.03) 0%, rgba(100, 200, 100, 0.08) 100%); border: 2px solid rgba(100, 200, 100, 0.2); position: relative; overflow: hidden;">
  <div style="position: absolute; top: -1px; left: 0; right: 0; height: 3px; background: linear-gradient(90deg, var(--grass-400), transparent);"></div>
  <h3 style="margin: 0 0 0.75rem 0; font-size: 1.1rem; color: var(--grass-400);">☁️ Manage Cloud Infrastructure</h3>
  <p style="margin: 0 0 1rem 0; font-size: 0.95rem; opacity: 0.8;">Control AWS, Azure, GCP resources with kubectl and GitOps-friendly infrastructure as code.</p>
  <strong>Start with:</strong> <a href="{{<ref "get-started/get-started-with-managed-resources">}}" style="color: var(--content-link-color); font-weight: 600; text-decoration: none;">Managed Resources Guide →</a>
  <p style="margin: 0.75rem 0 0 0; font-size: 0.85rem; opacity: 0.7; border-top: 1px solid rgba(100, 200, 100, 0.1); padding-top: 0.75rem;">Best for: DevOps, infrastructure as code, GitOps</p>
</div>

<div style="padding: 1.75rem; border-radius: 12px; background: linear-gradient(135deg, rgba(200, 150, 50, 0.03) 0%, rgba(200, 150, 50, 0.08) 100%); border: 2px solid rgba(200, 150, 50, 0.2); position: relative; overflow: hidden;">
  <div style="position: absolute; top: -1px; left: 0; right: 0; height: 3px; background: linear-gradient(90deg, var(--sun-400), transparent);"></div>
  <h3 style="margin: 0 0 0.75rem 0; font-size: 1.1rem; color: var(--sun-400);">⚙️ Automate Operations</h3>
  <p style="margin: 0 0 1rem 0; font-size: 0.95rem; opacity: 0.8;">Run operational tasks and day-two workflows like monitoring, scaling, and maintenance.</p>
  <strong>Start with:</strong> <a href="{{<ref "get-started/get-started-with-operations">}}" style="color: var(--content-link-color); font-weight: 600; text-decoration: none;">Operations Guide →</a>
  <p style="margin: 0.75rem 0 0 0; font-size: 0.85rem; opacity: 0.7; border-top: 1px solid rgba(200, 150, 50, 0.1); padding-top: 0.75rem;">Best for: Monitoring, scaling, upgrades, compliance</p>
</div>

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

## Choose Your Learning Path

Crossplane has three core features. **Which one should you learn first?**

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1.5rem; margin: 2rem 0;">

<div style="padding: 1.5rem; border-radius: 8px; background: var(--body-background); border: 1px solid var(--border-color);">
  <h3 style="margin: 0 0 0.5rem 0; font-size: 1.1rem;">🎯 Build Custom APIs</h3>
  <p style="margin: 0 0 1rem 0; font-size: 0.95rem; opacity: 0.8;">Create platform abstractions by composing multiple resources</p>
  <strong>Start with:</strong> <a href="{{<ref "get-started/get-started-with-composition">}}">Composition Guide →</a>
  <p style="margin: 0.75rem 0 0 0; font-size: 0.85rem; opacity: 0.7;">Best for: Platform teams, IDPs, standardization</p>
</div>

<div style="padding: 1.5rem; border-radius: 8px; background: var(--body-background); border: 1px solid var(--border-color);">
  <h3 style="margin: 0 0 0.5rem 0; font-size: 1.1rem;">☁️ Manage Cloud Infrastructure</h3>
  <p style="margin: 0 0 1rem 0; font-size: 0.95rem; opacity: 0.8;">Control AWS, Azure, GCP resources with kubectl</p>
  <strong>Start with:</strong> <a href="{{<ref "get-started/get-started-with-managed-resources">}}">Managed Resources Guide →</a>
  <p style="margin: 0.75rem 0 0 0; font-size: 0.85rem; opacity: 0.7;">Best for: DevOps, infrastructure as code, GitOps</p>
</div>

<div style="padding: 1.5rem; border-radius: 8px; background: var(--body-background); border: 1px solid var(--border-color);">
  <h3 style="margin: 0 0 0.5rem 0; font-size: 1.1rem;">⚙️ Automate Operations</h3>
  <p style="margin: 0 0 1rem 0; font-size: 0.95rem; opacity: 0.8;">Run operational tasks and day-two workflows</p>
  <strong>Start with:</strong> <a href="{{<ref "get-started/get-started-with-operations">}}">Operations Guide →</a>
  <p style="margin: 0.75rem 0 0 0; font-size: 0.85rem; opacity: 0.7;">Best for: Monitoring, scaling, upgrades, compliance</p>
</div>

</div>

**Not sure which path?** Check [Foundations]({{<ref "foundations">}}) to understand core concepts first.


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
