There are three python programs here (-h for usage):

-`default` randomly tags the German sentences in the data-dev directory
-`baseline` is skeleton code for you to implement the direct projection algorithm
-`grade` scores your output against the pre-tagged references and gives an accuracy

Like previous homeworks, the commands work in a pipeline:

    > ./default | ./grade

Or:

    > ./baseline | ./grade

The data-dev directory contains the following data files:

-`comtrans-dev.a`: word alignments for the ComTrans German-English parallel corpus
-`comtrans-dev.g`: German sentences
-`comtrans-dev.e`: parallel English sentences
-`refs-dev.g`: pre-tagged version of the above German sentences for grading
-`refs-dev.e`: pre-tagged version of the English sentences to use with direct projection
