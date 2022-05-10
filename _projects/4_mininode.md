---
layout: page
title: mininode
description: a security tool to reduce attack surface of Node.js application
# img: assets/img/7.jpg
importance: 1
category: work
github: https://github.com/wspr-ncsu/mininode
---

Mininode is a CLI tool to reduce the attack surface of the Node.js applications by using static analysis of source code. It supports two modes of reduction (1) coarse, (2) fine.

Mininode constructs the dependency graph (modules and functions used) of the application starting from main file, i.e. entry point of the application. Mininode initializes entry point to package.json file's main field if it exists. Otherwise default to index.js.

Example usage: `node index.js <path to Node application root folder> --mode=(coarse|fine)`.