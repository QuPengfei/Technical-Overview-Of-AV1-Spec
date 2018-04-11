# Technical-Overview-Of-AV1-Spec
This will give a snapshot of coding tools in the AV1 Spec.

Abstract 
--------
AV1 (AOMedia Video Codec 1.0) evolved on the basis of VP9 (Google), Thor (Cisco) and Daala (Mozila) under the AOM (Alliance for Open Media). It includes a number of enhancement and the new tools that have been added to improve the coding efficiency. The new tools that are added so far include 4 main aspects: prediction, transform, in-loop filter and entropy encoder. This document provides a snapshot of the coding tools in the current finalized version (on March, 2018) of AV1 spec.

Introduction
--------
	According to the AOM web page, AV1 is designed with the following feature.
-	Royally free
-	Scales to any modern device at any bandwidth
-	For use in both commercial and non-commercial content, including user-generated content
-	Developed for the internet and related applications and services-from browsers and streaming to videoconferencing services
-	Designed with a low computational footprint and optimized for hardware
-	Bringing features like 4k UHD, HDR, and WCG to real-time video
