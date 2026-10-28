# PlantPal - AI Plant Identification and Personal Care Assistant

> A proposed web app that helps beginner plant owners in Malaysia identify plants, keep a plant collection, manage care reminders and understand care advice.

## Problem Statement

Beginner indoor-plant owners in Malaysia need help identifying plants, understanding care instructions and remembering care tasks. PlantPal will use AI to suggest plant names from photos and explain care information from reviewed documents. Users can confirm a plant, optionally save it to a private collection, record care and receive editable reminders to check their plants.

## Evidence of the Problem

- [University of Illinois Extension, Watering](https://extension.illinois.edu/houseplants/watering) explains how under- and overwatering harm houseplants and how water needs vary with the plant and its environment.
- [University of Tennessee Extension, Caring for Indoor Plants, W 1128-B](https://uthort.tennessee.edu/wp-content/uploads/sites/228/2023/11/W1128B.pdf) explains how containers, growing media and conditions affect care.

These sources support the care problem. Demand among Malaysian users and any improvement from PlantPal still need to be tested.

## Initial Related Work / Existing Approaches

| Study / existing solution | Existing approach | Limitation relevant to PlantPal |
|---|---|---|
| [Pl@ntNet](https://docs.plantnet.org/en/reference/api-plantnet/) | Suggests species from plant images. | A suggested name still needs confirmation and does not show soil moisture or care needs. |
| [Planta](https://getplanta.com/) | Offers plant identification, care reminders and plant organisation. | Similar features already exist; PlantPal must evaluate its own three-species coverage, source support and reminder controls. |
| [University of Illinois Extension houseplant guidance](https://extension.illinois.edu/houseplants/watering) | Provides expert plant-care information. | Readers still need to find relevant advice and manage their own records and reminders; some advice needs local-context review. |

## Engineering Gap / Limitation

PlantPal will bring photo identification, a personal plant collection, care reminders and clear care advice into one app. Users will confirm the suggested plant name before receiving guidance for it.

Similar apps already exist. This project will focus on making these features work well together and testing whether its AI answers are more helpful than searching the same documents.

## Project Objectives

- **O1 – Design:** Plan a system for three plant types that connects photo identification, user confirmation, saved plant records and care guidance from trusted sources.
- **O2 – Develop:** Build a mobile-friendly English web app where users can identify plants, save them, set care reminders, record care activities and ask questions.
- **O3 – Evaluate:** Test identification accuracy, answer correctness, source links, privacy, reminders, response speed and running costs. Compare the care assistant with a basic keyword search.

## Target Users

PlantPal is intended for beginner indoor-plant owners in Malaysia, including students and people living in small homes.

V1 is an English, mobile-friendly web app for three plants: golden pothos / money plant (*Epipremnum aureum*), snake plant (*Dracaena trifasciata*, also called *Sansevieria trifasciata*) and spider plant (*Chlorophytum comosum*).

## Data Sources

| Public document | Proposed use |
|---|---|
| [Money Plant Care Guide](https://wengthye.com/careguide/money-plant) | Care guidance for golden pothos. |
| [Snake Plant Care Guide](https://wengthye.com/careguide/snake-plant) | Care guidance for snake plants. |
| [Spider Plant Care Guide](https://wengthye.com/careguide/spider-plant) | Care guidance for spider plants. |

These three guides are published by 54 Weng Thye Nursery in Sungai Buloh, Selangor. Check the advice against an independent horticultural reference and review usage rights before using it. Give answers only when approved passages support them, including repotting questions.

## Weekly Progress

| Week | Focus | Deliverables | Status |
|---|---|---|---|
| W1 | Observation, system map and project scope | README and three Week 1 files | ✅ Completed |
| W2 | Requirements, evaluation and V1 architecture | Four Week 2 files and reflections | ✅ Planning deliverables completed |
