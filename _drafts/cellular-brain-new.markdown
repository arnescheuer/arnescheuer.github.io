---
layout: page
title:  "Cellular Brain"
img-path: "/assets/img/ai/cellular-brain/"
---

# A model based on cellular automata

## 1. Introduction

Traditionally, approaches to simulate human brain functions tended to rely on architectural designs directly inspired by the human anatomical model. Consider for example neural networks used to imitate learning processes. Here I aim to explore methods for constructing a cellular brain that fulfills the requirements of being a) generic in terms of functional use, and, b) simple in terms of state and rule definition. In fact, the model proposed below should work as a multipurpose information processor.

## 2. General model specification

The starting point of my approach stresses the fact that all sensual input (visual, oral, and so on) enters the human brain as a continued chain of signals. For our goal we can model this chain of signals as discrete input of sequences of characters. The cellular brain should be capable of transferring temporal succession and repetition of input words in the presence of noise into throughput and output patterns coded as sequences of characters with a similar syntax than the input.

The following concepts are used to implement this model:

### 2.1. Modeling environment

The model environment employed here technically represents a deterministic two dimensional cellular automaton. The following boundary conditions hold:

- Defined uniform environment concerning dimensions, neighborhood
- Defined position of input / output cells
- Defined restriction of number of characters that could be stored in one cell
- Overall black-box approach: input generates output without predefined transformation process

### 2.2. Formal grammar

The idea is that the cellular brain, working in black box like manner, transforms the input character sequences into a sequence of output characters. Each transformation is effectuated in accordance with the grammar specified below, but only the sequences finally passed to the output cells will be taken into consideration for calculating model performance.

Grammar in EBNF (Extended Backus-Naur Form):

{% include cb_grammar.html %}


### 2.3. Mealy automaton

The cellular brain thus could be conceived as a deterministic automaton that uses input and throughput to generate output after a countable sequence of steps. A rather simple relationship between cell activity level and cell information content is used to model the transfer of information through the cellular brain.

The following equations are used for this purpose:

{% include cb_rule_equations.html %}

for:

{% include cb_rule_variables.html %}


During model simulation runs, the equations are calculated in the above shown order for each cell. The behavior of the system could be summarized as follows: information creation determines activity creation and activity creation determines information transfer.

### 2.4. Intra-cellular information management

In contrast to activity, information does not constitute a homogeneous substance. Consequently, relative position of information tokens in a cell potentially matters. The management of information in a cell could thus be understood as a queuing mechanism: some aspects of model behavior like information fading and information substitution decrease the amount of cell information, other processes like information input and information transfer increase the amount of cell information.

Four different queuing mechanism have been implemented:

- “FIFO”: “first in - first out”, meaning that information will be added to and returned from the same end of the queue (say, the left end)
- “FILO”: “first in - last out”, meaning that information will be added to one end and returned from the other end of the queue.
- “FREQ”: The most “frequent” token will be added from information providing processes to the queue, and, also the most “frequent” token returned from the queue to information consuming processes
- “RARE”: The “rarest” token will be added from information providing processes to the queue, and, also the “rarest” token will be returned from the queue to information consuming processes

#### 2.4.1 Mixed queue

The mixed queue uses all four queuing mechanisms, depending on the “information saturation” of a cell at the moment of information substitution (compare equation (4)). This indicator of “information saturation” is calculated as follows:

{% include cb_rule_information_saturation.html %}

relates the total amount of gained information through input and transfer, divided firstly by the number of delivering cells (true neighbors for transfer and fictional neighbors for input) and, secondly, by the maximal information content of a cell. The indicator can attain values between 0 and 1. The higher the value, the higher the supply of new information to the cell. Different value intervals of the indicator of “information saturation” correspond to different queue mechanism usages:

{% include cb_rule_information_saturation.html %}

During a model simulation run the queuing mechanism is thus likely to change several times.

#### 2.4.2. Pure “FILO” queue

As the name suggests, this queue exclusively relies on the “FILO” mechanism.

## 3. Association capability

My first goal will be to test the association capability of the model. In my opinion, association constitutes one of most astonishing competences of the human brain and could be regarded as a prerequisite for a number of key cognitive abilities like knowledge, memory, evaluation, reasoning. Association goes well beyond the simple linkage of temporal or spatial coincidence. Complex, seemingly subconscious reasoning patterns result in surprising connections between isolated events.

### 3.1. Measurement of associative content

In data mining applications, some algorithms are popular for detecting for association patterns between elements of item sets. This constitutes one of the most important tasks for example in the analysis of shopping behavior. My approach tries to refine the classical methodology with special consideration of temporal relationship between individual elements (in our case tokens of the character input sequences) and of noise with no obvious significance for the detection of association patterns. As already stated in the conditions defining the modeling environment, one cell could only hold a limited number of character. In order to allow unrestricted access to the input cells of cellular brain, the length of individual sentences should not be surpass the storage limit.

#### 3.1.1. Association filtering quotient

The first algorithm used to evaluate the associative power of the cellular brain comprises the following steps:

1. Enter a first input text into the defined input cell of the brain (here it is assumed that only one input and one output cell will used during the measurement process), one sentence during each iteration.
2. Let the brain work with the first input text and throughput for a certain number of iterations
3. Measure the associative content of the second input text:
3.1. Ignore all tokens that appear with a high frequency above a certain limit, for example more than 50% of all tokens of a single sentence. Goal: eliminate noise.
3.2. Detect any pair of different tokens that appear in the text, count their pair wise frequency and evaluate the distance between any pair wise appearance of tokens. The lesser the distance and the higher the frequency, the higher the resulting associative score of the token pair. Note: order is NOT relevant, so the following two different token combinations A-B and B-A are treated as instances of the same pair.
3.3. Calculate the sum of all pair wise associative scores.
4.Enter the second input text into the defined input cell of the brain (here it is assumed that only one input and one output cell will used during the measurement process), one sentence during each iteration.
5.Let the brain work with the input and throughput for a certain number of iterations.
6. Save the content of the defined output cell of the brain over a certain number of iterations, corresponding to the number of iterations necessary to enter the input into the brain.
7. Measure the associative content of the output text using the same procedure as in step 1.
8. Calculate the quotient of output and input associative content. The higher the quotient, the better the associative power of the cellular brain.

#### 3.1.2. Association recombination rate

In contrast to the association filtering quotient, the association recombination rate does not put so much emphasis on the measurement of association conservation, as indicated by temporal succession, but on possible re-combination of elementary tokens.

The recombination rate value is thus calculated using the following procedure:

1. Enter a first input text into the defined input cell of the brain (here it is assumed that only one input and one output cell will used during the measurement process), one sentence during each iteration.
2. Let the brain work with the first input text and throughput for a certain number of iterations
3. Measure the associative content of the second input text:
3.1. Ignore all tokens that appear with a high frequency above a certain limit, for example more than 50% of all tokens of a single sentence. Goal: eliminate noise.
3.2. Detect any pair of different tokens that appear in the text and evaluate the distance between any pair wise appearance of tokens. Store temporarily the smallest distance of all the occurrences of one pair. As noted already above, order is NOT relevant.
4. Enter the second input text into the defined input cell of the brain (here it is assumed that only one input and one output cell will used during the measurement process), one sentence during each iteration.
5. Let the brain work with the input and throughput for a certain number of iterations.
6. Save the content of the defined output cell of the brain over a certain number of iterations, corresponding to the number of iterations necessary to enter the input into the brain.
7. Measure the associative content of the output text applying the following logic to each token pair found:
7.1. Set score to 1
7.2. Check if one of the following cases applies:
7.2.1. If both token stem from second input, but if tokens are found in different input sentences, multiply score by input sentence distance;
7.2.2. Increase score by factor 2, if pair is not found in second input
7.3. Divide resulting score by factor 2, if token are found in different output sentences
8. Add up all pair wise scores

#### 3.1.3. Association lift quotient

The association lift quotient is conceptually related to the association filtering quotient. Output and input text sequences are analyzed separately in a first step, and than related to one another in order to measure the development of associative content. The "lift" concept is frequently used in classical association analysis: 

1. Enter a first input text into the defined input cell of the brain (here it is assumed that only one input and one output cell will used during the measurement process), one sentence during each iteration.
2. Let the brain work with the first input text and throughput for a certain number of iterations
3. Measure the associative content of the second input text:
3.1. Do NOT ignore all tokens that appear with a high frequency above a certain limit, for example more than 50% of all tokens of a single sentence.
3.2. Detect any pair of different tokens that appear in the text, count their pair wise frequency and evaluate the distance between any pair wise appearance of tokens. The lesser the combined distance and the higher the frequency, the higher the resulting associative score of the token pair. As noted already above, order is N OT relevant.
3.3. Calculate the global sum of all pair wise associative scores.
3.4. Calculate the LIFT for each detected pair:
3.4.1. Calculate SCORE A-B: take the above calculated score sum of individual pairs A-B and B-A, divided by distance in sentences.
3.4.2. Calculate SCORE X: take the above calculated global score sum.
3.4.3. Calculate SCORE A: summing up occurrence of all pairs A-X + X-A, divided by distance measured in sentences.
3.4.4. Calculate SCORE B: summing up occurrence of all pairs B-X + X-B, divided by distance measured in sentences..
3.4.5. Calculate SUPPORT A-B: SCORE A-B divided by SCORE X.
3.4.6. Calculate SUPPORT A: SCORE A divided by SCORE X.
3.4.7. Calculate SUPPORT B: SCORE B divided by SCORE X.
3.4.8. Calculate LIFT A-B: SUPPORT(A-B) / SUPPORT(A) * SUPPORT(B).
3.5. Sort pairs by descending order of LIFT.
4. Enter the text into the defined input cell of the brain (here it is assumed that only one input and one output cell will used during the measurement process), one sentence during each iteration.
5. Let the brain work with the input and throughput for a certain number of iterations.
6. Save the content of the defined output cell of the brain over a certain number of iterations, corresponding to the number of iterations necessary to enter the input into the brain.
7. Measure the associative content of the output text:
7.1. Eliminate output token pairs that have no connection to input pairs, meaning that both tokens of the individual pair do not appear in the second input text.
7.2. Calculate the LIFT for each remaining pair as described above under 3 and sort the pairs in descending order of LIFT.
8. Calculate the quotient of output and input associative content: sum up the LIFT for the three highest scoring output pairs and divide by the LIFT sum for the three highest scoring pairs of second input.

### 3.2. Experiment 1: Variation of activity fading rate

#### 3.2.1. Specific model settings

- Cell update procedure: according to the above sequence of equations
- Habitat: 12 \* 3 = 36 cells
- Cell information capacity: 100 characters (including token separator “-”)
- Information fading coefficient $i <sub>fading</sub>$ = 0.25
- Correction factor for information transfer due to activity gradient i creation transfer, = not included
- Activity gradient limit for information transfer a barrier transfer = not included
- Correction factor for information content inhibiting information transfer i barrier transfer = not included
- Correction factor for activity creation via information input a creation input = 1
- Correction factor for activity creation via information collision a creation collision = 1
- Correction factor for activity creation via information transfer a creation transfer = 1
- Queue type: mixed queue
- Iterations: 40
- Input cell: central cell of leftmost column
- Output cell: central cell of rightmost column
- Input: “A-B-C-D-E” at iteration 1 to 5; “V-W-X-Y-Z” at iteration 11 to 12
- Output measurement lag in relation to second input sequence: 25 iterations
- Visualization: dark colors indicate low activity levels, bright colors higher levels

#### 3.2.2. Results

##### 3.2.2.1. Case 1: activity fading rate = 0.123

- Output at iteration 35 and 36: -
- Association filtering quotient: 0
- Mean activity level per cell: 0.002
- Information token distribution: A:0.94/V:0.021/B:0.021/C:0.021

![Experiment 1 - Case 1]({{page.img-path}}graph1.jpg)

##### 3.2.2.2. Case 2: activity fading rate = 0.122

- Output at iteration 35 and 36: “A” \* 58 => string with 29 times character “A” per measurement iteration
- Association quotient: 0
- Mean activity level per cell: 0.939
- Information token distribution: A:1.0

[Experiment 1 - Case 2]({{page.img-path}}graph2.jpg)

#### 3.2.3. Discussion

- In both cases, the employed model specified does not show the intended behavior: associations between the two successive input sequences were not constructed or at least not transported to the output cell.
- In case 1, information transfer comes to a standstill. Activity levels and gradients do not allow for further proliferation.
- In case 2, activity levels stay high until the end of the simulation period. But the information transfer process does not favor the conservation of information diversity. Majority rule single out one token from the first input sequence.
- The most interesting fact from the experiment concerns the sudden, system wide change in activity levels after an incremental variation of the activity fading rate. Obviously an internal threshold was reached, an indication of non linear behavior of the system as a whole.

### 3.3. Experiment 2: Effect using uniform information queuing mechanism

#### 3.3.1.Specific model settings

- Cell update procedure: according to the sequence of equations described in section 2.3
- Habitat: 12 \* 3 = 36 cells
- Cell information capacity: 10 characters (including token separator "-")
- Correction factor for information transfer through activity gradient i creation transfer, = between 0.7 and 1.3, incremented in steps of 0.3
- Activity gradient limit for information transfer a barrier transfer = between 0.2 and 0.8, incremented in steps of 0.1
- Correction factor for information content inhibiting information transfer i barrier transfer = between 0.2 and 0.8, incremented in steps of 0.1
- Information fading coefficient i fading between 0.2 and 0.8, incremented in steps of 0.1
- Correction factor for activity creation via information input a creation input between 0.7 and 1.3, incremented in steps of 0.3
- Correction factor for activity creation via information collision a creation collision between 0.7 and 1.3, incremented in steps of 0.3
- Correction factor for activity creation via information transfer a creation transfer between 0.7 and 1.3, incremented in steps of 0.3
- Activity fading coefficient a fading between 0.2 and 0.8, incremented in steps of 0.1.
- Queue type: FILO queue
- Iterations: 40
- Input cell: central cell of leftmost column
- Output cell: central cell of rightmost column
- Input: "A-B-C-D-E" at iteration 1 to 5; "V-W-X-Y-Z" at iteration 11 to 12
- Output measurement lag in relation to second input sequence: 25 iterations
- Measurement of model performance: association filtering quotient, association lift quotient, association recombination rate

#### 3.3.2. Justification of model setting changes

- Parameter variation: For parameters that proved to be especially influential in experiment 1, a narrow variation range reflects the knowledge already obtained about the range of decisive values. New parameters not used in experiment 1 are given special emphasis with a higher number of increments.
- Queue type: information transfer and information substitution in the cells is now, in contrast to the first experiment, modeled using the pure FILO (first-in, last-out) queue, as described in chapter 2.4.2. The formerly employed mixed queue, as described in chapter 2.4.1., uses different substitution and transfer rules, depending on the total amount of information ready for transfer in the neighborhood of each cell and in the queue itself. In fact, the algorithm tends to cause a quick majority rule selection effect on the different input tokens.

#### 3.3.3. Experiment design

- Individual model simulations for each possible parameter value combination using the intervals described in section 3.3.1. All in all 194481 simulations (7\*7\*7\*3\*3\*3\*3\*7) are conducted.
- The results of those model runs with at least one performance indicator exhibiting non-zero value, are considered further in the course of the experiment. These simulations are considered as the population whole for further statistical analysis.
- With the help of correlation analysis, the three measurement concepts for model performance are checked for redundancy.
- The values of the 8 parameters varied during the simulations will be standardized to make them comparable. For this purpose a z-transformation renders a value distribution with mean zero and standard deviation 1 for each parameter.
- The model run results are put in descending order of model performance. For each performance indicator, those simulations with goodness of fit above certain thresholds (level of the mean plus two or three standard deviations) are put into separate samples.
- Those parameters are identified whose values show a marked different distribution in the sub-samples with high model fit, in comparison with the value distribution in all runs with at least minimal model fit. T-tests are employed to compare both distributions for all parameters.

#### 3.3.4. Results

- A proportion of 0.37 (72930 simulations) of all model runs attains above-zero model fitness, thus forming the population whole for the following statistical analysis.
- The association filtering quotient and the association lift quotient are highly correlated with a coefficient of 0.82. This means they effectively represent identical concepts. The association lift quotient is preferred for further usage, because it does not filter out noise. Thus the special lift concept that considers expectation values, does favor unlikely and non-trivial individual associations between tokens.
- In contrast, the association recombination indicator is not correlated with both other measuring concepts: coefficient 0.02 for correlation with filtering quotient, coefficient -0.15 for correlation with lift quotient. Therefore this concept is retained for further usage.
- For both remaining measurement indicators, samples for those simulations with indicator values above a) mean plus 2 standard deviations and b) mean plus 3 standard deviations, are drawn. Concerning the lift quotient, no individual model runs attain values above the threshold of mean plus 3 standard deviations. As a result, only three samples of simulation types with high goodness of fit must be considered.
- The following tables visualizes the properties of the samples of simulation runs with high model performance, obtained by using different performance indicators and different threshold values for performance indicator values in the individual model runs:

##### Table 3.3.4.1: Arithmetic mean of parameter values in samples with high modeling performance (mean in whole population always 1)

{% include cb_experiment_2_table1.html %}

##### Table 3.3.4.2: T-test values indicating differences between population whole and samples (values in brackets are not significant on the 0.01 level) 

{% include cb_experiment_2_table1.html %}

#### 3.3.5. Discussion

- Overall, the uniform information queuing mechanism tends to smooth out the extreme effect of small parameter value changes observed in experiment 1. In contrast to experiment 1, a high percentage of all simulations resulted in non-zero association indicator values.
- Concerning the relative importance of the parameters, high T-test values indicate that the high performance sub-samples are significantly influences by the parameter in question.
- In general, the association recombination indicator is more sensitive to parameter variation.
- Both fading rates show especially significant T-test values. This seams plausible, because they determine the persistence potential of information in individual cells. The means of both parameters exhibit comparatively low values in all three sub-samples with high model fitness.
- The information transfer parameter has no influence at all on the association performance. It could be argued that this parameter does not discriminate between cells. All cells are treated alike and so the relative influence on model performance is leveled out by other parameters determining locally specific information transfer, namely those modifying activity creation by input or collision, and those who cause transfer barriers to be erected or removed.

{% include licence.html %}
