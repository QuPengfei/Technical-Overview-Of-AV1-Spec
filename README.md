Technical Overview of AV1 Spec
====================================

Abstract 
=========

AV1 (AOMedia Video Codec 1.0) evolved on the basis of VP9 (Google), Thor (Cisco)
and Daala (Mozila) under the AOM (Alliance for Open Media). It includes a number
of enhancement and the new tools that have been added to improve the coding
efficiency. The new tools that are added so far include 4 main aspects:
prediction, transform, in-loop filter and entropy encoder. This document
provides a snapshot of the coding tools in the current finalized version (on
March, 2018) of AV1 spec.

Introduction
============

According to the AOM web page, AV1 is designed with the following feature.

-   Royally free

-   Scales to any modern device at any bandwidth

-   For use in both commercial and non-commercial content, including
    user-generated content

-   Developed for the internet and related applications and services-from
    browsers and streaming to videoconferencing services

-   Designed with a low computational footprint and optimized for hardware

-   Bringing features like 4k UHD, HDR, and WCG to real-time video

Profile & Levels
================

Profiles and levels specify restrictions on the capabilities needed to decode
the bitstreams. The profile specifies the bit depth and subsampling formats
supported, while the level defines resolution and performance characteristics.
By now levels is still under discussion and there is no more details.

AV1 support the three named profiles as the table list.

| Profile | Bit depth | Monochrome support | Chroma subsampling  | Name         |
|---------|-----------|--------------------|---------------------|--------------|
| 0       | 8/10      | Yes                | 4:2:0               | Main         |
| 1       | 8/10      | No                 | 4:4:4               | High         |
| 2       | 8/10      | Yes                | 4:2:2               | Professional |
| 2       | 12        | Yes                | 4:2:0, 4:2:2, 4:4:4 | Professional |

Table 1. AV1 Profile

Block Structure
===============

Basic Coding block
------------------

AV1 support the larger super block size, which is up to 128x128 super block is
allowed. It supports from 128x128 down to 4x4 coding block. Each 4x4 luma block
is allowed to independently select inter or intra mode, its reference mode, and
interpolation filter type. For Chroma, 2x2 block size is allowed but still 4x4
transform block size is used.

Basic Prediction Block
----------------------

AV1 support up to 10 partition type. The size of partition unit is allowed down
to 4x4 and totally there are 24 types of block size.

| Partition index | Type of partition |
|-----------------|-------------------|
| 0               | PARTITION_NONE    |
| 1               | PARTITION_HORZ    |
| 2               | PARTITION_VERT    |
| 3               | PARTITION_SPLIT   |
| 4               | PARTITION_HORZ_A  |
| 5               | PARTITION_HORZ_B  |
| 6               | PARTITION_VERT_A  |
| 7               | PARTITION_VERT_B  |
| 8               | PARTITION_HORZ_4  |
| 9               | PARTITION_VERT_4  |

Table 2. Type of Block partition

| Index | Partition Block size | Index | Partition Block size |
|-------|----------------------|-------|----------------------|
| 0     | BLOCK_4X4            | 12    | BLOCK_64X64          |
| 1     | BLOCK_4X8            | 13    | BLOCK_64X128         |
| 2     | BLOCK_8X4            | 14    | BLOCK_128X64         |
| 3     | BLOCK_8X8            | 15    | BLOCK_128X128        |
| 4     | BLOCK_8X16           | 16    | BLOCK_4X16           |
| 5     | BLOCK_16X8           | 17    | BLOCK_16X4           |
| 6     | BLOCK_16X16          | 18    | BLOCK_8X32           |
| 7     | BLOCK_16X32          | 19    | BLOCK_32X8           |
| 8     | BLOCK_32X16          | 20    | BLOCK_16X64          |
| 9     | BLOCK_32X32          | 21    | BLOCK_64X16          |
| 10    | BLOCK_32X64          | 22    | BLOCK_32X128         |
| 11    | BLOCK_64X32          | 23    | BLOCK_128X32         |

Table 3. Size of Block Partition

Basic Transform Block
---------------------

Both square and rectangle transform block size is supported in AV1. There are
total 19 transform block size.

| Index | TxSize   | Index | TxSize   |
|-------|----------|-------|----------|
| 0     | TX_4X4   | 10    | TX_32X16 |
| 1     | TX_8X8   | 11    | TX_32X64 |
| 2     | TX_16X16 | 12    | TX_64X32 |
| 3     | TX_32X32 | 13    | TX_4X16  |
| 4     | TX_64X64 | 14    | TX_16X4  |
| 5     | TX_4X8   | 15    | TX_8X32  |
| 6     | TX_8X4   | 16    | TX_32X8  |
| 7     | TX_8X16  | 17    | TX_16X64 |
| 8     | TX_16X8  | 18    | TX_64X16 |
| 9     | TX_16X32 |       |          |

>   Table 4. Size of Transform Block

Intra Prediction
================

Intra Prediction in AV1 expends largely compared to VP9. Here is snapshot of
Intra Mode.

| Index | Intra mode          | AV1 | VP9 | Comments                                      |
|-------|---------------------|-----|-----|-----------------------------------------------|
| 0     | DC_PRED             | X   | X   |                                               |
| 1     | V_PRED              | X   | X   | AV1 support 7 kind of mode based on this mode |
| 2     | H_PRED              | X   | X   | AV1 support 7 kind of mode based on this mode |
| 3     | D45_PRED            | X   | X   | AV1 support 7 kind of mode based on this mode |
| 4     | D135_PRED           | X   | X   | AV1 support 7 kind of mode based on this mode |
| 5     | D113_PRED           | X   | X   | AV1 support 7 kind of mode based on this mode |
| 6     | D157_PRED           | X   | X   | AV1 support 7 kind of mode based on this mode |
| 7     | D203_PRED           | X   | X   | AV1 support 7 kind of mode based on this mode |
| 8     | D67_PRED            | X   | X   | AV1 support 7 kind of mode based on this mode |
| 9     | SMOOTH_PRED         | X   |     |                                               |
| 10    | SMOOTH_V_PRED       | X   |     |                                               |
| 11    | SMOOTH_H_PRED       | X   |     |                                               |
| 12    | TM_PRED(PAETH_PRED) | X   | X   | AV1 replace TM_PRED with PAETH_PRED           |
| 13    | Palette Mode        | X   |     |                                               |

Table 5. Summary of Intra Mode between AV1 and VP9

Directional Intra Prediction Mode
---------------------------------

VP9 only supports 8 directional intra prediction modes: D45_PRED, D63_PRED,
H_PRED, D117_PRED, D135_PRED, D153_PRED, V_PRED, D207_PRED. These modes
correspond to prediction angles of 45, 63, 90, 117, 135, 153, 180, and 207
degrees, respectively.

To improve intra coding efficiency, more prediction angle options are added to
AV1. The prediction angle is calculated as the following:

Prediction angle = nominal_angle + (angle_delta \* angle_step),

| nominal_angle                       | angle_step | angle_delta | Total number of angles |
|-------------------------------------|------------|-------------|------------------------|
| 45, 63, 90, 117, 135, 153, 180, 207 | 3          | [-3, +3]    | 8\*7=56                |

Table 6. Finer of Intra Mode

-   norminal_angle is determined by the prediction mode, and is the same as VP9;

-   angle_delta is in a predefined range and angle_step is a predefined value.
    In current configuration, angle_delta is in the range of [-3, +3] and
    angle_step is 3. These settings are selected experimentally.

-   The total number of supported prediction angles is therefore increased from
    8 to 8 \* 7 = 56.

Smooth Mode
-----------

It is a Non- Directional Intra Prediction mode. VP9 has 2 non-directional intra
prediction modes: DC_PRED and TM_PRED. AV1 expands on this by adding 3 new
smooth prediction modes: SMOOTH_PRED, SMOOTH_V_PRED and SMOOTH_H_PRED. The new
modes work as follows:

|Mode|Comments|
|-|-|
| SMOOTH_PRED   | Useful for predicting blocks that have a smooth gradient. It works as follows: estimate the pixels on the rightmost column with the value of the last pixel in top row, and estimate the pixels in the last row of the current block using the last pixel in left column. Then calculate the rest of the pixels by an average of quadratic interpolation in vertical and horizontal directions, based on distance of the pixel from the predicted pixels. |
| SMOOTH_V_PRED | Similar to SMOOTH_PRED, but uses quadratic interpolation only in the vertical direction                                                                                                                                                                                                                                                                                                                                                                   |
| SMOOTH_H_PRED | Similar to SMOOTH_PRED, but uses quadratic interpolation only in the horizontal direction                                                                                                                                                                                                                                                                                                                                                                 |

Table 7. Smooth mode of Intra mode

Paeth Mode
----------

It is a Non- Directional Intra Prediction mode. The new prediction mode
PAETH_PRED replaces the existing mode TM_PRED.

TM_PRED: Predictor(TM) = left + top – top_left

PAETH_PRED: Predictor (PAETH) = argmin \|x- Predictor(TM)\|

The idea is to find out the One of left, top, top_left closest in value to
Predictor(TM).

Palette Mode
------------

Sometimes, given intra block can be approximated by a block with small number of
unique colors. This is especially true for artificial videos like
screen-capture, games etc. For such cases, AV1 introduces a new intra coding
mode called palette mode. This predictor for a block is signaled by storing (i)
a color palette, with 2 to 8 colors, and (ii) color indices into the palette for
all pixels in the block. The residual pixel values of the block are as usual
transformed and quantized before being entropy-coded.

Palette mode can be used by both intra-only as well as inter frames. The number
of base colors determines the trade-off between fidelity and compactness. The
color indices for pixels are obtained by the nearest neighbor method. The color
indices are encoded using the neighborhood-based context to be as compact as
possible.

Palette Mode is not new. We can see the Palette Mode and Intra block copy in the
HEVC SCC (Screen Content Coding) extension.

Filter Intra mode
-----------------

AV1 adopt the new mode to interpolate (intra filter) the reference samples
before prediction. This will reduce the impact of quantization noise. Here is
the table to specify the type of intra filtering.

| Index | Filter intra type |
|-------|-------------------|
| 0     | INTRA_DC_PRED     |
| 1     | INTRA_V_PRED      |
| 2     | INTRA_H_PRED      |
| 3     | INTRA_D153_PRED   |
| 4     | INTRA_TM_PRED     |

Table 8 Type of Intra filter Mode

Intra Block Copy Mode
---------------------

This tool is very efficient for coding of screen content video in that repeated
patterns in text and graphics rich content occur frequently within the same
picture. Having a previously reconstructed block with equal or similar pattern
as a predictor can effectively reduce the prediction error and therefore improve
coding efficiency.

In AV1, Intra block copy is only allowed in intra frames. It disables all loop
filtering and only integer offsets are allowed in block copy mode.

Predict Chroma from Luma
------------------------

Chroma from luma (CfL) prediction is a new and promising chroma-only intra
predictor that models chroma pixels as a linear function of the coincident
reconstructed luma pixels.

Inter Prediction
================

Affine/Warped Motion Compensation
---------------------------------

Traditional modern codecs, including VP9, use block motion compensation where
motion vectors are translational only. This is not sufficient for real video
which often contains complex motion. For example, motion due to camera shake,
panning and zoom might require transformations that support shearing, scaling,
rotation and changes in aspect ratio. In AV1, we introduce warped motion
compensation implemented as similarity and affine transformations to better
capture the diversity of motion that exists in real video. There are two
affine/warped motion compensation.

| Affine Motion Compensation | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Global                     | It is common for videos to contain a global camera motion which is pertinent to an entire inter frame. It is therefore beneficial to transmit a set of motion parameters at the frame level that is applicable to a large number of blocks in the frame. When a frame is encoded, a set of global motion parameters is computed and transmitted between that frame and each reference frame. These parameters may be either translational, similarity or affine motion model. Subsequently, any block in the frame can signal use of the global motion mode with a given reference to create a suitable predictor. |
| Local                      | Affine motion compensation is also useful to describe complex local object motion. Here, we estimate affine parameters for a single block using the translational motion vectors that are typically conveyed for all inter blocks. Specifically, we estimate an affine or similarity model using the motion vectors from the current block and its causal neighbors which share the same reference frame.                                                                                                                                                                                                          |

Table 9 Affine Motion Compensation

OBMC (Overlapped Block Motion Compensation)
-------------------------------------------

Motions assigned to surrounding blocks will contribute to predicting a current
block, via a well-defined overlapping scheme appropriately designed for advanced
variable block-size partitioning frameworks.

The OBMC will blend multiple predictors from neighbor blocks. It is not new
concept and was proposed and implemented back in the era of h.263. The OBMC was
proved to largely reduce prediction errors but not adopted by recent codecs due
to extra complexity in the scenario of hybrid inter/intra variable block size
coding. In AV1, a practical overlapping mechanism based on two-stage 1-D
filtering is proposed for the advanced partitioning framework to implement
causal overlapped block prediction.

Sub-pixel Interpolation Filter
------------------------------

The motion vector used in modern video codecs is allowed to have a fractional
position for a better prediction quality. So, an interpolation filter module is
needed to generate the prediction block at a fractional position in the
reference frame. VP9 codec uses a separable interpolation filter to perform
inter prediction with ⅛ motion vector precision. Three filter types, SHARP,
REGULAR and SMOOTH, in descending order of cutoff frequencies, are provided to
deal with various types of noise/distortions that can occur in reference
frames/blocks. Given a filter type and a motion vector, the interpolation filter
is performed by two one-dimensional filters, one for horizontal direction and
one for vertical direction.

In AV1 codec, dual interpolation filter is introduced on top of the
interpolation module inherited from VP9. Dual filter allows each block/frame to
use a different interpolation filter type in horizontal and vertical direction.
Up to 9 types of filter will be applied to the block.

This idea is based on the observation that a reference frame/block’s horizontal
and vertical signals may have distinct frequency characteristics; therefore,
using different filter types may produce a better prediction. As before, both
the filter types are transmitted in the bitstream on a per block or per frame
basis.

At the same time AV1 use the high intermediate precision between the horizontal
and vertical filter. The same high precision before average the predictors with
compound mode.

Dynamic MV reference
--------------------

VP9 has two candidates MV in the ref list and 4 type of mode (NEARESTMV, NEARMV,
NEWMV, and ZEROMV) are used. AV1 support 4 candidate MV and more modes.

For single ref mode, AV1 is same as VP9.

For compound mode, VP9 restricts motion vectors for a compound predictor to
share one motion vector referencing mode, even though they may use different
reference frames. To add more flexibility, on top of existing four combinations
(NEAREST_NEARESTMV, NEAR_NEARMV, NEW_NEWMV, ZERO_ZEROMV) in VP9, AV1 supports
four more empirically selected combinations: NEAREST_NEWMV, NEW_NEARESTMV,
NEAR_NEWMV, and NEW_NEARMV.

| Index | Type                         | Ref Mode        |
|-------|------------------------------|-----------------|
| 0     | NEARESTMV                    | single ref mode |
| 1     | NEARMV                       | single ref mode |
| 2     | GLOBALMV(ZEROMV)             | single ref mode |
| 3     | NEWMV                        | single ref mode |
| 4     | NEAREST_NEARESTMV            | compound mode   |
| 5     | NEAR_NEARMV                  | compound mode   |
| 6     | NEAREST_NEWMV                | compound mode   |
| 7     | NEW_NEARESTMV                | compound mode   |
| 8     | NEAR_NEWMV                   | compound mode   |
| 9     | NEW_NEARMV                   | compound mode   |
| 10    | GLOBAL_GLOBALMV(ZERO_ZEROMV) | compound mode   |
| 11    | NEW_NEWMV                    | compound mode   |

Table 10 MV mode

Extended Compound Modes
-----------------------

AV1 Compound mode support both predictors from the same direction and VP9 only
support from the different direction (One forward and one backward reference
frame). VP9 only support 1/2 weight to blend the two predictor and AV1 support
more flexible weight blending.

| Index | Compound type     | Comments                                                                                                                              |
|-------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| 0     | COMPOUND_WEDGE    | Inter-Inter Wedge mode Inter-Intra Wedge mode                                                                                         |
| 1     | COMPOUND_SEG      | Inter-Inter Compound Segment mode                                                                                                     |
| 2     | COMPOUND_AVERAGE  | (1/2,1/2) weight will be applied to blend the predictors                                                                              |
| 3     | COMPOUND_INTRA    | Inter-Intra Gradual mode                                                                                                              |
| 4     | COMPOUND_DISTANCE | This process computes weights to be used for blending predictions together based on the expected output times of the reference frames |

Table 11. Compound type

Here are more details about the Compound Segment Mode:

-   Inter-Inter Compound Segment mode

In many cases, regions in one predictor will contain useful content that is not
present in the other. The two inter predictors have a larger pixel difference
generally.

-   Inter-Inter Wedge mode

Boundaries of moving objects in a video often separate two regions with distinct
motions. Coding these regions with separate motion vector reference combinations
should be beneficial; however, finding exact object boundaries is not only
difficult, but expensive to communicate in the bitstream. Our approach is to
design a codebook of masks with only a few possible partitioning combinations
and signaling the codebook index in the bitstream.

The AV1 wedge codebook contains partition orientations that are either
horizontal, vertical or oblique with slopes: 2, -2, 0.5 and -0.5. The wedge
prediction mode is used for all square and rectangular blocks, using the 16-ary
shape codebooks.

| Index | Wedge direction  | Comments |
|-------|------------------|----------|
| 0     | WEDGE_HORIZONTAL |          |
| 1     | WEDGE_VERTICAL   |          |
| 2     | WEDGE_OBLIQUE27  |          |
| 3     | WEDGE_OBLIQUE63  |          |
| 4     | WEDGE_OBLIQUE117 |          |
| 5     | WEDGE_OBLIQUE153 |          |

Table 12. Wedge direction

-   Inter-Intra Gradual mode

Decay the weight gradually for the intra from the prediction boundary and
increase the weight of inter correspondingly. It support four modes, which
include horizontal mode, vertical mode, DC_PRED, and SMOOTH_PRED.

-   Inter-Intra Wedge mode

Blocks cannot always perfectly partition moving objects. For example, occlusion
can occur in the middle of a block, it is better to apply different prediction
techniques to different contents. Contents that are not occluded in reference
frame will prefer inter prediction, while newly revealed content could benefit
more from intra prediction using local reference.

Extended Reference frame Number
-------------------------------

Up to 7 reference frames out of 8 in the frame stored buffer are extended to be
used in the inter mode. The reference frames is allowed to come from the same
side or different side in the AV1.

LAST3_FRAME, LAST2_FRAME and LAST_FRAME are forward references and LAST_FRAME is
the near past frame. BWDREF_FRAME is a backward reference, similar to
ALTREF_FRAME.

Here is the table to show the reference frame type.

| Index | Ref frame Name |
|-------|----------------|
| 0     | INTRA_FRAME    |
| 1     | LAST_FRAME     |
| 2     | LAST2_FRAME    |
| 3     | LAST3_FRAME    |
| 4     | GOLDEN_FRAME   |
| 5     | BWDREF_FRAME   |
| 6     | ALTREF2_FRAME  |
| 7     | ALTREF_FRAME   |

Table 13 Reference frame type

In-loop Filter
==============

Several in-loop tools in AV1 are employed. De-blocking, CDEF and loop
restoration are cascaded.

De-blocking filter
------------------

AV1 support 4 filter levels per frame and VP9 only has one. Two levels are for
Luma component (horizontal and vertical levels). The other two levels are for U
and V component separately. In AV1, filter level is allowed to change superblock
by superblock.

CDEF (Constrained Directional Enhancement Filter)
-------------------------------------------------

CDEF is the combination of CLPF (Constrained Low Pass Filter) and Deringing
filter. The main goal of the in-loop CEDF is to filter the coding artifacts and
ringing while preserving the detail of image. It takes into account the
direction of edge and patterns in the image. It is the similar to the SAO of
HEVC.

The CDEF is based on the following observation. The amount of ringing artifacts
in a coded image tends to be roughly proportional to the quantization step size.
The amount of detail is a property of the input image, but the smallest detail
actually retained in the quantized image tends to also be proportional to the
quantization step size. For a given quantization step size, the amplitude of the
ringing is generally less than the amplitude of the details.

CDEF works as the following steps:

-   The frame is divided into filter blocks of 64x64 pixels. Some CDEF
    parameters are signaled at the frame level, and some may be signaled at the
    filter block level.

-   To identify the direction of edge or pattern in each filter block.

-   To adaptively filter along the identified direction and to a lesser degree
    along directions rotated 45 degrees from the identified direction. The
    filter strengths are signaled explicitly, which allows a high degree of
    control over the blurring.

The main reason for identifying the direction is to align the filter taps along
that direction to reduce ringing while preserving the directional edges or
patterns. CDEF defines primary taps and secondary taps filter. The primary taps
follow the direction and the secondary taps form a cross, oriented 45 off the
direction. Both primary and secondary taps filter have 8 types.

LR (In-loop Restoration) filter
-------------------------------

AV1 employ a set of in-loop image restoration tool after de-blocking to
generally de-noise and enhance the quality of the edge. In-loop restoration
scheme have two types of filter to remove blur artifacts due to block
processing. One is Wiener Filter. The other is Dual Self-Guided filter. These
tools are integrated into AV1 with a switchable framework, which trigger the
different tool in the different image region.

Multi-Symbol Entropy Coder
==========================

Multi-symbol adaptive arithmetic coding model is adopted in AV1. Both syntax
element and coefficient are coded with this model.

Most recent video codecs encode information using binary arithmetic coding, such
as CABA or CAVLC in AVC/HEVC, meaning that each symbol can only take two values.
The AV1 entropy encoder come from the Daala range coder and supports up to 16
values per symbol, making it possible to encode fewer symbols. This is
equivalent to coding up to four binary values in parallel and reduces serial
dependencies, allowing hardware implementations to use lower clock rates, and
thus less power.

Transform
=========

Transform type
--------------

For AV1, there is a richer set of transforms for coding Inter and Intra
prediction residues. Inter prediction residues do not have a well-defined
structure as in the Intra case, but using a bank of transforms, each adapted to
a specific type of residue profile within the block, is generally helpful.

In AV1, four types of transform are used mainly in the horizontal and vertical
direction separately. The total 16 different transforms are available.

| Transform type | Comments                                                                                                                                                 |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| DCT            | Inter and Intra modes continue to make use of DCT.                                                                                                       |
| ADST           | Asymmetric Discrete Sine Transform                                                                                                                       |
| Flip ADST      | It applies ADST in reverse order                                                                                                                         |
| IDTX           | Identity transform seems to be particularly useful for coding residue with sharp lines and edges. Identity transform is useful for screen content coding |

Table 14 The Main Transform Type in each of direction

For each small coded block (4x4 or 8x8), it is possible to choose one of up to
16 different transforms as follows(Detail in Table):

{DCT, ADST, FlipADST, IDTX} horizontal x {DCT, ADST, FlipADST, IDTX} vertical

As block sizes get larger, some of these transforms begin to act similarly.
Thus, a reduced set of transforms is used for 16x16, 32x32 and 64x64 block
sizes. In the transform selection process for Inter and Intra modes, the encoder
does a search over the entire set of transforms and selects the one that
produces the best rate-distortion cost. Once a transform is selected, a
transform type symbol from the set of types available at that size is used to
indicate the actual transform used in the bitstream.

There are 6 types of transform sets in the AV1 spec, which specify the transform
type of Intra and Inter blocks. The transform sets determine what subset of
transform types can be used, according to the following table.

| Inter or not | Set Number | Transform set  |
|--------------|------------|----------------|
| Don’t care   | 0          | TX_SET_DCTONLY |
| 0            | 1          | TX_SET_INTRA_1 |
| 0            | 2          | TX_SET_INTRA_2 |
| 1            | 1          | TX_SET_INTER_1 |
| 1            | 2          | TX_SET_INTER_2 |
| 1            | 3          | TX_SET_INTER_3 |

Table 15 Transform Set in the AV1 spec

| Transform type    | TX_SET_DCTONLY | TX_SET_INTRA_1 | TX_SET_INTRA_2 | TX_SET_INTER_1 | TX_SET_INTER_2 |
|-------------------|----------------|----------------|----------------|----------------|----------------|
| DCT_DCT           | X              | X              | X              | X              | X              |
| ADST_DCT          |                | X              | X              | X              | X              |
| DCT_ADST          |                | X              | X              | X              | X              |
| ADST_ADST         |                | X              | X              | X              | X              |
| FLIPADST_DCT      |                |                |                | X              | X              |
| DCT_FLIPADST      |                |                |                | X              | X              |
| FLIPADST_FLIPADST |                |                |                | X              | X              |
| ADST_FLIPADST     |                |                |                | X              | X              |
| FLIPADST_ADST     |                |                |                | X              | X              |
| IDTX              |                | X              | X              | X              | X              |
| V_DCT             |                | X              |                | X              | X              |
| H_DCT             |                | X              |                | X              | X              |
| V_ADST            |                |                |                | X              |                |
| H_ADST            |                |                |                | X              |                |
| V_FLIPADST        |                |                |                | X              |                |
| H_FLIPADST        |                |                |                | X              |                |

Table 16 Detailed Transform type supported in each transform set.

Transform Block Shape and Size
------------------------------

Both square and rectangle shape block are used in AV1. The transform block size
is less than the partition block size. The block size is very flexible and up to
64x64 and down to 4x4. Details see the table in the Block section.

Tiles
=====

AV1 support flexible tiles, which include uniform and non-uniform tile spacing.
Tile area is limited to a maximum 4096x2304. Tiles can be grouped into tile
group and each group can be decoded independently to achieve error resilience.
Loop filter can be enabled or disabled across tiles.

Segment
=======

Same as VP9, AV1 provides a means of segmenting the image and then applying
various adjustments at the segment level. Up to 8 segments may be specified for
any given frame. For each of these segments it is possible to specify:

- A quantizer (absolute value or delta).

- A loop filter strength (absolute value or delta).

- A prediction reference frame.

- A block skip mode that implies both the use of a (0,0) motion vector and that
no residual will be coded.

SVC (Scalable Video Coding)
===========================

AV1 support temporal and spatial layer coding. Temporal layer support up to 8
layers and spatial layer support up to 3 layers.

| Index | Scalability mode | Index  | Scalability mode  |
|-------|------------------|--------|-------------------|
| 0     | SCALABILITY_L1T2 | 8      | SCALABILITY_L2T2h |
| 1     | SCALABILITY_L1T3 | 9      | SCALABILITY_L2T3h |
| 2     | SCALABILITY_L2T1 | 10     | SCALABILITY_S2T1h |
| 3     | SCALABILITY_L2T2 | 11     | SCALABILITY_S2T2h |
| 4     | SCALABILITY_L2T3 | 12     | SCALABILITY_S2T3h |
| 5     | SCALABILITY_S2T1 | 13     | SCALABILITY_SS    |
| 6     | SCALABILITY_S2T2 | 14-255 | reserved          |
| 7     | SCALABILITY_S2T3 |        |                   |

Table 17. Temporal and Spatial Mode

| Scalability mode  | Spatial Layers | Resolution Ratio | Temporal Layers | Inter-layer-dependency |
|-------------------|----------------|------------------|-----------------|------------------------|
| SCALABILITY_L1T2  | 1              |                  | 2               |                        |
| SCALABILITY_L1T3  | 1              |                  | 3               |                        |
| SCALABILITY_L2T1  | 2              | 2:1              | 1               | Yes                    |
| SCALABILITY_L2T2  | 2              | 2:1              | 2               | Yes                    |
| SCALABILITY_L2T3  | 2              | 2:1              | 3               | Yes                    |
| SCALABILITY_S2T1  | 2              | 2:1              | 1               | No                     |
| SCALABILITY_S2T2  | 2              | 2:1              | 2               | No                     |
| SCALABILITY_S2T3  | 2              | 2:1              | 3               | No                     |
| SCALABILITY_L2T2h | 2              | 1.5:1            | 2               | Yes                    |
| SCALABILITY_L2T3h | 2              | 1.5:1            | 3               | Yes                    |
| SCALABILITY_S2T1h | 2              | 1.5:1            | 1               | No                     |

Table 17. Details in the Temporal and Spatial Mode


Other Tools
===========

Quantization Matric
-------------------

AV1 support 15 sets of QMs, which are based on the contrast-sensitive functions.
QMs are applied to a frame based on selectable scaling of its quantization
level, higher level of quantization imply flatter matrices. The matrices become
flatter as the quantization index value increases (and the quality decreases).
Inter matrices are slightly flatter than intra matrices.

Superblock Delta-quantization
-----------------------------

AV1 allow the per-superblock changes in quantization parameter to support
sub-frame rate control. At the same time it support the ROI level rate control
on the top of segmentation level parameter.

OBU (Open Bitstream Unit)
-------------------------

An AV1 bitstream consists of a number of OBUs that are normally held within a
container format alongside audio and timing information. Here the new tool OBU
is introduced in AV1 and it is similar to NAL (Network Abstract Layer) in
AVC/HEVC spec.

The OBU header is similar to the NAL header. In general the total 8 bits are
presented. The OBU extra 8 bits of extension header is used if temporal and
spatial layer exist in the bitstream. obu_type is the most important syntax to
describe the type of OBU .

| Index | obu_type                   |
|-------|----------------------------|
| 0     | Reserved                   |
| 1     | OBU_SEQUENCE_HEADER        |
| 2     | OBU_TD                     |
| 3     | OBU_FRAME_HEADER           |
| 4     | OBU_TILE_GROUP             |
| 5     | OBU_METADATA               |
| 6     | OBU_FRAME                  |
| 7     | OBU_REDUNDANT_FRAME_HEADER |
| 8-14  | Reserved                   |
| 15    | OBU_PADDING                |

Table 18. Type of OBU

Reference:
==========

1.  <https://aomediacodec.github.io/av1-spec/av1-spec.pdf>

