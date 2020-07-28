---
title: "ELI5: Kubernetes Operators"
date: 2020-07-26
draft: false
toc: true
# images:
# tags:
#   - untagged
---
## Introduction

I've been thinking about starting an ELI5 (Explain It Like I'm Five) Computer Science blog for a while now. I enjoy simplifying obscure, abstract concepts and making them intuitive and accessible. A wise friend once told me that complexity is easy; simplicity is hard. I hope this post succeeds in that goal.

If not, I can always stick to my day job. 

## The Kubernetes Restaurant

Let's say that you (a user) just walked into a restaurant named ([Kubernetes](https://kubernetes.io/)). You're seated at a table, given a menu, and, after an appropriate length of time, a charming waiter (the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)) takes your order, which consists of several items ([Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/)). 

The waiter adds your order to the order list ([etcd](https://kubernetes.io/docs/concepts/overview/components/#etcd), the Kubernetes data store).

A grizzled, curmudgeonly cook (a [Kubernetes controller](https://kubernetes.io/docs/concepts/architecture/controller/)) retrieves your order from the order list and prepares the requested dishes.

Note, you don't actually have to know how the food is prepared. That's the nice thing about restaurants. Sometimes you feel like eating something, but you don't have the time, skills, or knowledge necessary to prepare it yourself. At this restaurant, all you have to do is state your desire **declaratively**, and the cook (the Kubernetes controller) gives you what you want by walking through a sequence of **imperative** steps. That's a big part of the beauty of Kubernetes. Many other solutions are the equivalent of cooking at home. Kubernetes offloads that imperative burden. 

## Operators

Let's say you're itching for a specific dish that's not on the menu. Luckily, the owner of the restaurant is open to suggestions from customers (Kubernetes is [awesomely extensible](https://kubernetes.io/docs/concepts/extend-kubernetes/extend-cluster/)). You can ask the owner to add an item to the menu (a [Custom Resource Definition](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/), which creates both a new object type and a new API endpoint). The owner generously agrees.

Now anyone can place an order for the new item (a [Custom Resource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)). The trouble is, the existing cooks (vanilla controllers) don't know how to prepare the new dish, so a new cook (a custom controller) is hired (deployed). When requests come in for the new order, the new cook responds to them by executing a sequence of steps and, in most cases, delegating some work to the original cooks (vanilla controllers). 

That's all an [Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) is: a new item on the menu and a new cook who knows how to prepare it, or in other words, the combination of a Custom Resource Definition and a custom controller that watches and responds to changes in the associated custom resources. 

## Conclusion

I'm not feeling very optimistic, but I hope you enjoyed this post. Maybe I'll add some terrible illustrations down the line. If nothing else, it beats how CRDs are described as horrific Dr.Moreau-esque chimeras in [Phippy Goes To The Zoo](https://www.cncf.io/phippy-goes-to-the-zoo-book/).

> Zee halted abruptly. In the distance, a black-railed fence arose. The arches above the pen were marked C-R-D. Between the bars, Zee could make out some peculiar critters. A giraffe with a hippopotamus head. A snake with raccoon ears. A lion with a beaver’s tail. A unicorn with no horn. Zee wasn’t sure she liked the looks of that place.

![](https://www.cncf.io/wp-content/uploads/2018/12/phippy-goes-to-the-zoo-18-1.png)

> “Oh,” said Phippy, a look of concern on her face, “Uh… look! It’s lunch time! We’d better head home.”

![](/img/phippy.png)

Calm down, Phipphy!
