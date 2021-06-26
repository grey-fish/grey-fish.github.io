# Cronos
> Name: Cronos
> Difficulty: Medium
> Solved: 25th June 2021

## Intro
Cronos is a Linux (ubuntu) machine which i found interesting to solve. Below i'll log how i approached it and what more could be done.

## Enumeration
Lets first scan the box with nmap to see services running. I used below command:
    `$ nmap -sC -sV -oA nmap/cronos 10.10.10.13`
Above command runs nmap with default scripts (-sC), version detection (-sV) and creates output in various file formats.

The scan gives the following output:
![](./images/cronos_nmap)

We have services running on port 22, 53 and 80. We'll check them in detail.

