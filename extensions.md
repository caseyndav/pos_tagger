	Extension 1 exploits unique attributes of the German language in order to overcome inherent deficiencies that arise from the usage of an alignment-based POS projection system.
	The basic alignment system does not produce perfect results because one-to-many and many-to-one alignments can result in faulty or absent tags. This extension was created with the goal of instituting an additional method to tag words that would be more accurate than just aligning them. One of the team members noted that every noun in the German language is capitalized, and examinations of the output revealed that at least one noun had been improperly labeled. Therefore, we knew we could create a program that treated capitalized words in a different manner and improve our results.
	In the baseline version of the code, the system examines a German word and gives it the POS tag of the corresponding English word to which it is aligned. If there is no corresponding word, or the actual word exists but has no tag, then the German word is tagged with an 'X.' This extension overrides the normal process by changing the usual word tagger to an elif statement and placing a new if statement before it. That if statement checks whether the word is at the front of the sentence through the use of a previously generated iterating variable that represents the current position, and also checks whether the first letter of the word is capitalized via thirty Boolean statements, one for each possible capital letter and including letters that only appear in the German language. The use of a large number of Booleans may seem unnecessary, but in fact it avoids common bugs, is one of if not the only way to check Unicode characters in an ASCII based system, and one of the authors of the code already had a similar piece written out that they could just copy over. The if statement passes only if the word is capitalized but is not the first word of the sentence, which would obviously be capitalized regardless of its POS. Keen or pedantic readers may have noted that this means the first word of the sentence is never affected by our extension, and this is both true and utterly irrelevant because changing it to tag that word in a special manner would be beyond the scope of this particular extension. Inside the new if statement is yet another nested one that checks if the capitalized word in question is 'Sie', 'Ihnen', or 'Ihr'. These are also German words that are always capitalized, but they are actually pronouns. This final check determines whether the word is tagged as a noun or pronoun, and then the actual tagging occurs. If the first if statement was not passed, the word is tagged in accordance with the English word to which it is aligned as normal. All of these details are heavily commented within the new code.
	When tested on a small 47-word sample of the data, the extension increased the percentage of correct tags from 58.924485 to 64.302059.
	When tested on the entirety of the data, the extension increased the percentage of correct tags from 58.87 to 63.47.


	Extension 2 makes assumptions about the nature of similar or identical entities in paired English and German sentences that may in fact result in some additional incorrect tags but which was found to have a net positive influence.
	This extension was created to address the same faults in the basic alignment system as the others. It was readily apparent that certain concepts that were identical in both versions of the sentences, such as 'h.', place names, and numbers, would have the same POS tags. However, examination of the output once again revealed that this was not the case. This extension addresses the problem by explicitly tagging those tokens with identical POS's. A logical continuation of this idea was causing sufficiently similar words to be tagged in the same manner. English and German are both members of the West Germanic language family, so we knew they would have large numbers of cognates.
	The extension's code takes on a similar form to that of extension 1, with only the structure of the new if statement changing. This version calculates the similarity of two words using an add-on module called Levenshtein that provides a 'distance' function. Words that are similar enough are tagged with the same POS as their identical or similar counterpart. The level of acceptable similarity can be set using the command line. The code also contains a specific exception that prevents it from treating certain words as identical. These words happen to exist in both German and English but have different meanings in each, and it would not do to have them be matched. Sadly, eliminating all false cognates would be a monumental task outside the scope of this extension, so some words may be accidentally matched. A full and official citation of our source for those words can be found as a comment inside the extension itself.
	When tested on a small 47-word sample of the data with the similarity threshold set to 0, the extension increased the percentage of correct tags from 58.924485 to 59.839817.
	When tested on a small 47-word sample of the data with the similarity threshold set to 1, the extension increased the percentage of correct tags from 58.924485 to 60.411899.
	When tested on a small 47-word sample of the data with the similarity threshold set to numbers higher than one, the percentage of correct tags continually dropped.
	When tested on the entirety of the data with the similarity threshold set to 0, the extension increased the percentage of correct tags from 58.87 to 59.98.
	When tested on the entirety of the data with the similarity threshold set to 1, the extension increased the percentage of correct tags from 58.87 to 60.18.
	When tested on the entirety of the data with the similarity threshold set to 2, the extension increased the percentage of correct tags from 58.87 to 56.04.
	When tested on the entirety of the data with the similarity threshold set to 3, the extension decreased the percentage of correct tags from 58.87 to 43.76.


	Extension 3 directly labels punctuation and numbers using a list of each. The baseline program commonly mislabels punctuation because it is often grouped in one-to-many and many-to-one alignments, and these errors are likely to be the first one noticed when manually examining the data.
	When tested on the entirety of the data, the extension increased the percentage of correct tags from 58.87 to 60.51.


	Extension 4 refines the tag outputs by first calculating the probability that each unique word will have a given tag and then reassigning "X" tags if the probability of the new tag is above a certain threshold.
	When tested on the entirety of the data, the extension increased the percentage of correct tags from 58.87 to 65.98.


	Our team also experimented with a fusion of extensions 1, 2, and 4 in order to produce the best possible result. The program was not submitted with the rest of the code due to the lack of an appropriate category in which to place it, but is mentioned here in order to complete the analysis of our extensions. The absence of extension 3 from this amalgamation occurs because while extension 3 constitutes a fundamentally different approach to the problem, both ideologically and in terms of how it was implemented, it does not make any changes to the output that do not also occur in extension 2. Furthermore, it would not be possible for extension 3's natural avoidance of the possible mistakes made by extension 2 to be implemented in the fused expansion. However, it could be said to have some degree of influence over the final product because the section of the code taken from extension 2 actually came from an older, Levenshteinless version that is only triggered by identical words and never ones that are merely similar.
	The program had two small changes from the extensions it was based upon. The first was the aforementioned alteration that ensured only identical words were affected by extension 2's code. The second was the placement of the relevant if statements after the code that tags words based on their alignment. This meant that only words that failed to be tagged were altered by the extensions. This aspect, a necessity in extension 4 but absent from the others, was found to cause most extensions to produce worse data but induced a half-percentage jump here.
	When tested on a small 47-word sample of the data, the extension increased the percentage of correct tags from 58.924485 to 68.535469. Another combination that only used extensions 1 and 2 and kept the new if statements in front of the old one correctly tagged 65.217391 percent of the data.
	When tested on the entirety of the data, the extension increased the percentage of correct tags from 58.87 to 70.42. It was indeed our best result.