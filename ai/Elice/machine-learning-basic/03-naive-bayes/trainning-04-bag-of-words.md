
```python
import re

special_chars_remover = re.compile("[^\w'|_]")

def main():
	sentence = input()
	bow = create_BOW(sentence)
	
	print(bow)


def create_BOW(sentence):
	bow = {}
	sentence_lowered = sentence.lower()
	sentence_without_sc = remove_special_characters(sentence_lowered)
	split_words = sentence_without_sc.split()
	filtered_split_words = [
		word
		for word in split_words
		if len(word) >= 1
	]

	for word in filtered_split_words:
		bow.setdefault(word, 0)
		bow[word] += 1

	return bow

  
def remove_special_characters(sentence):
	return special_chars_remover.sub(' ', sentence)


if __name__ == "__main__":
	main()
```