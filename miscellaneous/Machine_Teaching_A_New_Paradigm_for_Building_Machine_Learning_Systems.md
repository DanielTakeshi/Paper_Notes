# Machine Teaching: A New Paradigm for Building Machine Learning Systems

From Microsoft Research, views machine teaching from the perspective of making
teachers more productive at building machine learning models.  So this is
probably a more generic definition than the one summarizing the NIPS 2017
machine teaching workshop.

Major points:

- Teaching can be done by example, by providing labels, "schema constraints",
  various features, and so forth. (I'd also add curriculum learning on data
  ordering, though they argue that CL is actually a hybrid for ML and MT.)

- Machine teaching is like programming in many ways (long analogy). View ML
  algorithms like neural nets as compilers that give us the desired function in
  some set. OK ...

- Decomposition is a hugely important skill, necessary for machine teaching in
  large part because software engineers have applied this skill successfully.
  Also probably related to their concept hierarchy example on the botanical
  gardens (though I think they could have had a better example). The example,
  though, raises the point of teachers changing their minds, which could happen!

- Software engineers have learned how to manage projects with lots of
  contributors (version control, APIs, etc.). How to do this for many people who
  need to use a machine learning algorithm?

Also on teachers:

The role of the teacher is to transfer knowledge to the learning machine so that
it can generate a useful model that can approximate a concept. 

> Definition 4.1 (Concept). A concept is a mapping from any example to a label
> value.

> Definition 4.2 (Feature). A feature is a concept that assigns each example a
> scalar value.

> Definition 4.3 (Teacher). A teacher is the person who transfers concept
> knowledge to a learning machine.

etc. Lots of relation to this paper and "Interpretable and Pedagogical Examples"
by the way!

Important point: teachers do not automatically know the program in their mind
when they try and label stuff, but subconsciously evaluate features, etc.
(Think of me, how do I look at something and know that it's in object class X?
There's no program.) If there was a program, we could write it and not need
machine teaching.

Also important:

> A key characteristic of domain experts is that they understand the semantics
> of a problem. To this point, we argue that if a problem’s data does not need
> to be interpreted by a person to be useful, machine teaching is not needed.
> For example, problems for which the labeled data is abundant or practically
> limitless; e.g. Computer Vision, Speech Understanding, Genomics Analysis,
> Click-Prediction, Financial Forecasting. For these, powerful learning
> algorithms or hardware may be the better strategy to arrive at an effective
> solution. In other problems like the above, feature selection using cross
> validation can be used to arrive at a good solution without the need of a
> machine teacher.

but:

> There is nonetheless, an ever-growing set of problems for which machine
> teaching is the right approach; problems where unlabeled data is plentiful and
> domain knowledge to articulate a concept is essential.

Well, unlabeled data is certainly an issue in robotics, though albeit some of
this is rectified by self-supervision which automatically labels.

They view Deep Learning as "behaviorism" learned solely from (input, output)
pairs, but are skeptical this can work for certain tasks (e.g., writing
Shakespeare solely from input/output pairs).
