# Technical-Overview-Of-AV1-Spec

This will give a snapshot of coding tools in the AV1 Spec.

## Abstract 

AV1 (AOMedia Video Codec 1.0) evolved on the basis of VP9 (Google), Thor (Cisco) and Daala (Mozila) under the AOM (Alliance for Open Media). It includes a number of enhancement and the new tools that have been added to improve the coding efficiency. The new tools that are added so far include 4 main aspects: prediction, transform, in-loop filter and entropy encoder. This document provides a snapshot of the coding tools in the current finalized version (on March, 2018) of AV1 spec.

## Introduction

According to the AOM web page, AV1 is designed with the following feature.
-	Royally free
-	Scales to any modern device at any bandwidth
-	For use in both commercial and non-commercial content, including user-generated content
-	Developed for the internet and related applications and services-from browsers and streaming to videoconferencing services
-	Designed with a low computational footprint and optimized for hardware
-	Bringing features like 4k UHD, HDR, and WCG to real-time video

## Profile & Levels

Profiles and levels specify restrictions on the capabilities needed to decode the bitstreams. The profile specifies the bit depth and subsampling formats supported, while the level defines resolution and performance characteristics. By now levels is still under discussion and there is no more details.
AV1 support the three named profiles as the table list.

|Profile|	Bit depth|	Monochrome support|	Chroma subsampling|	Name|
|-|-|-|-|-|
|0|	8/10|	Yes|	4:2:0|	Main|
|1|	8/10|	No|	4:4:4|	High|
|2|	8/10|	Yes|	4:2:2|	Professional|
|2|	12|	Yes|	4:2:0, 4:2:2, 4:4:4|	Professional|

Table 1. AV1 Profile

## Block Structure

### Basic Coding block
AV1 support the larger super block size, which is up to 128x128 super block is allowed. It supports from 128x128 down to 4x4 coding block. Each 4x4 luma block is allowed to independently select inter or intra mode, its reference mode, and interpolation filter type. For Chroma, 2x2 block size is allowed but still 4x4 transform block size is used.
Basic Prediction Block
AV1 support up to 10 partition type. The size of partition unit is allowed down to 4x4 and totally there are 24 types of block size.

|Partition index|	Type of partition|
|-|-|
|0|	PARTITION_NONE|
|1|	PARTITION_HORZ  |
|2|	PARTITION_VERT|
|3|	PARTITION_SPLIT|
|4|	PARTITION_HORZ_A|
|5|	PARTITION_HORZ_B|
|6|	PARTITION_VERT_A|
|7|	PARTITION_VERT_B|
|8|	PARTITION_HORZ_4|
|9|	PARTITION_VERT_4|

Table 2. Type of Block partition


|Index	|Partition Block size	|Index	|Partition Block size|
|-|-|-|-|
|0	|BLOCK_4X4	|12	|BLOCK_64X64	|
|1	|BLOCK_4X8	|13	|BLOCK_64X128	|
|2	|BLOCK_8X4	|14	|BLOCK_128X64	|
|3	|BLOCK_8X8	|15	|BLOCK_128X128	|
|4	|BLOCK_8X16	|16	|BLOCK_4X16	|
|5	|BLOCK_16X8	|17	|BLOCK_16X4	|
|6	|BLOCK_16X16	|18	|BLOCK_8X32	|
|7	|BLOCK_16X32	|19	|BLOCK_32X8	|
|8	|BLOCK_32X16	|20	|BLOCK_16X64	|
|9	|BLOCK_32X32	|21	|BLOCK_64X16	|
|10	|BLOCK_32X64	|22	|BLOCK_32X128	|
|11	|BLOCK_64X32	|23	|BLOCK_128X32	|

Table 3. Size of Block Partition

### Basic Transform Block
Both square and rectangle transform block size is supported in AV1. There are total 19 transform block size.

|Index	|TxSize	|Index	|TxSize		|
|-|-|-|-|
|0	|TX_4X4	|10	|TX_32X16	|
|1	|TX_8X8	|11	|TX_32X64	|
|2	|TX_16X16|12	|TX_64X32	|
|3	|TX_32X32|13	|TX_4X16	|
|4	|TX_64X64|14	|TX_16X4	|
|5	|TX_4X8	|15	|TX_8X32	|
|6	|TX_8X4	|16	|TX_32X8	|
|7	|TX_8X16	|17	|TX_16X64	|
|8	|TX_16X8	|18	|TX_64X16	|
|9	|TX_16X32|	|		|

Table 4. Size of Transform Block
