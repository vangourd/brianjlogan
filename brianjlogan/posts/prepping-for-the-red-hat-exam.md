.. title: Prepping for the Red Hat Exam
.. slug: prepping-for-the-red-hat-exam
.. date: 2020-02-07 11:36:09 UTC-05:00
.. tags: rhce
.. category: linux
.. link: 
.. description: My first post about the RHCE exam
.. type: text

![Red Hat](https://media-exp1.licdn.com/dms/image/C4E0BAQEto-TydTTIfQ/company-logo_200_200/0?e=2159024400&v=beta&t=vzgZDtu_hRuaw6XxrMkAHqaHC4I88iZPO5hrFjoTtp0)

I have decided to go after the [RCHE/RCHSA Certificate](https://www.redhat.com/en/services/certification/rhcsa). 

I will be posting some details about my studying for the course under my tag for [RCHE](http://localhost:8000/categories/rhce/).

<!-- TEASER_END -->

Some initial notes:
[CentOS](https://www.centos.org/) is a rebuild distro of RHEL that allows for circumventing all that pesky commercial licensing hooplah.

I made the mistake of going for Centos v8 but you should not that the exam is on RHEL7 not RHEL8 and there are some...

## ...Differences you might need to account for.

One major thing that I noticed is that the **Acaconda text installer doesn't support manual partitioning in v7 but does in v8**. I also noticed that on v8 trying to do an http install to KVM fails with some generic *dracut* message. You can try and hash it out on an emergency shell but why bother? For me using a v7 image resolved the problem immediately. This might be specific to me but I tried on a Lenovo ThinkServer and a Dell Optiplex 9020 both suppliers have quite good linux support and had the issue across both hardwares. 

## Back to the two hardwares

It has been enlightening for me to work [through my study guide](https://www.oreilly.com/library/view/rhcsarhce-red-hat/9780071841948/) on two different systems. Reading & working together on the first system, then *catching* the other system up from memory might have some [spaced repetition influences](https://en.wikipedia.org/wiki/Spaced_repetition) but more importantly tests those skills on disparate setups. You might need to problem solve those differences for the exam or any future setup for that matter. 