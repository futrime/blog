---
author: Futrime
title: What's Conway's Game of Life
description: These days I was addicted to a new type of "game", Conway's Game of Life. And I've written a webpage of it.
date: 2020-10-10T07:20:11Z
categories:
    - Experience
    - Logging
tags:
    - Game of Life
    - Conway
draft: false
---

## My Project

The project is now available on Gitee, a GitHub alternative in China, which also provides Pages service that I put the program on it. If interested, you can fork it and make any contribution to it. However, due to some problems, now the project can only be maintained in Chinese. The project is on [Futrime/Game-of-Life](https://github.com/futrime/Game-of-Life). If only want to experience the game, please access [DEMO](https://futrime.github.io/Game-of-Life/).

The program is purely written in HTML and Javascript, but also use [dat.GUI](https://github.com/dataarts/dat.gui)

Here is the introduction adapted from [Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

## Introduction

The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970.[1] It is a zero-player game, meaning that its evolution is determined by its initial state, requiring no further input. One interacts with the Game of Life by creating an initial configuration and observing how it evolves. It is Turing complete and can simulate a universal constructor or any other Turing machine.

## Rules

The universe of the Game of Life is an infinite, two-dimensional orthogonal grid of square cells, each of which is in one of two possible states, live or dead, (or populated and unpopulated, respectively). Every cell interacts with its eight neighbours, which are the cells that are horizontally, vertically, or diagonally adjacent. At each step in time, the following transitions occur:

1. Any live cell with fewer than two live neighbours dies, as if by underpopulation.
1. Any live cell with two or three live neighbours lives on to the next generation.
1. Any live cell with more than three live neighbours dies, as if by overpopulation.
1. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

These rules, which compare the behavior of the automaton to real life, can be condensed into the following:

1. Any live cell with two or three live neighbours survives.
1. Any dead cell with three live neighbours becomes a live cell.
1. All other live cells die in the next generation. Similarly, all other dead cells stay dead.

The initial pattern constitutes the seed of the system. The first generation is created by applying the above rules simultaneously to every cell in the seed; births and deaths occur simultaneously, and the discrete moment at which this happens is sometimes called a tick. Each generation is a pure function of the preceding one. The rules continue to be applied repeatedly to create further generations.

References:
1. https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life
1. https://futrime.gitee.io/game-of-life/
1. https://gitee.com/futrime/game-of-life
1. https://github.com/dataarts/dat.gui
