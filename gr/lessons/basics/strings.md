---
layout: page
title: Αλφαριθμητικά
category: basics
order: 14
lang: gr
---

Αλφαριθμητικά, Χαρακτήρες Λιστών, Γραφήματα και Κωδικοσημεία.

{% include toc.html %}

## Αλφαριθμητικά

Τα αλφαριθμητικά στην Elixir δεν είναι τίποτα άλλο από αλληλουχία bytes.  Ας δούμε ένα παράδειγμα:

```elixir
iex> string = <<104,101,108,108,111>>
"hello"
```

>Σημείωση: Η χρήση του συντακτικού << >> δηλώνει στο μεταγλωτιστή ότι τα στοιχεία μέσα σε αυτά τα σύμβολα είναι bytes.

## Λίστες Χαρακτήρων

Εσωτερικά, τα αλφαριθμητικά στην Elixir αναπαριστώνται με μια αλληλουχία bytes αντί ενός πίνακα χαρακτήρων.  Η Elixir επίσης έχει έναν τύπο λιστας χαρακτήρων.  Τα αλφαριθμητικά στην Elixir περικλείονται από διπλά εισαγωγικά, ενώ οι λίστες χαρακτήρων με μονά.

Ποιά η διαφορά;  Κάθε τιμή στη λίστα χαρακτήρων είναι η τιμή ASCII του χαρακτήρα.  Πιο αναλυτικά:

```elixir
iex> char_list = 'hello'
'hello'

iex> [hd|tl] = char_list
'hello'

iex> {hd, tl}
{104, 'ello'}

iex> Enum.reduce(char_list, "", fn char, acc -> acc <> to_string(char) <> "," end)
"104,101,108,108,111,"
```

Όταν προγραμματίζουμε στην Elixir, συνήθως χρησιμοποιούμε Αλφαριθμητικά, όχι λίστες χαρακτήρων. Η υποστήριξη για λίστες χαρακτήρων υπάρχει επειδή χρειάζεται σε μερικές ενότητες της Erlang.

## Γραφήματα και Κωδικοσημεία

Τα κωδικοσημεία είναι απλά χαρακτήρες Unicode, οι οποίοι μπορεί να αναπαριστώνται από ένα ή δύο bytes.  Για παράδειγμα, οι χαρακτήρες με περισπωμένη ή τόνους: `á, ñ, è, ά`. Τα γραφήματα αποτελούνται από πολλαπλά κωδικοσημεία που δείχνουν σαν ένας απλός χαρακτήρας.

Η ενότητα Αλφαριθμητικών (String) παρέχει δύο μεθόδους για την παραγωγή τους, τις `graphemes/1` και `codepoints/1`.  Για παράδειγμα:

```elixir
iex> string = "\u0061\u0301"
"á"

iex> String.codepoints string
["a", "́"]

iex> String.graphemes string
["á"]
```

## Συναρτήσεις Αλφαριθμητικών

Ας δούμε μερικές από τις πιο σημαντικές και χρήσιμες συναρτήσεις της ενότητας String.  Αυτό το μάθημα θα καλύψει ένα υποσύνολο των διαθέσιμων συναρτήσεων, για να δείτε το πλήρες σετ συναρτήσεων επισκεφθείτε τα επίσημα έγγραφα της [`String`](http://elixir-lang.org/docs/stable/elixir/String.html).

### `length/1`

Επιστρέφει το σύνολο των Γραφημάτων στο αλφαριθμητικό.

```elixir
iex> String.length "Γεια"
4
```

### `replace/3`

Επιστρέφει ένα νέο αλφαριθμητικό αντικαθιστώντας ένα τρέχον πρότυπο στο αλφαριθμητικό με ένα αλφαριθμητικο αλλαγής.

```elixir
iex> String.replace("Hello", "e", "a")
"Hallo"
```

### `duplicate/2`

Επιστρέφει ένα νέο αλφαριθμητικό επαναλαμβανόμενο n φορές.

```elixir
iex> String.duplicate("Oh my ", 3)
"Oh my Oh my Oh my "
```

### `split/2`

Επιστρέφει μία λίστα αλφαριθμητικών χωρισμένη από ένα πρότυπο.

```elixir
iex> String.split("Γεια σου κόσμε", " ")
["Γειά", "σου", "κόσμε"]
```

## Ασκήσεις

Ας δούμε μερικές απλές ασκήσεις για να αποδείξουμε ότι είμαστε έτοιμοι με τα αλφαριθμητικά!

### Αναγραματισμοί

Το Α και το Β θεωρούνται αναγραματισμοί αν υπάρχει ένας τρόπος να αλλάξουμε το Α και το Β κάνοντάς τα ίσα.  Για παράδειγμα:

+ Α = τεστ
+ B = τσετ

Αν αλλάξουμε σειρά στους χαρακτήρες του αλφαριθμητικού Α, θα πάρουμε το αλφαριθμητικό Β και αντίστροφα.

Οπότε, πως θα μπορούσαμε να ελέγξουμε αν δύο αλφαριθμητικά είναι Αναγραματισμοί στην Elixir;  Η πιό εύκολη λύση είναι να ταξινομήσουμε τα γραφήματα κάθε αλφαριθμητικού αλφαβητικά και να ελέγξουμε αν και οι δύο λίστες είναι ίσες.  Ας το δοκιμάσουμε:

```elixir
defmodule Anagram do
  def anagrams?(a, b) when is_binary(a) and is_binary(b) do
    sort_string(a) == sort_string(b)
  end

  def sort_string(string) do
    string
    |> String.downcase
    |> String.graphemes
    |> Enum.sort
  end
end
```

Ας δούμε πρώτα την `anagrams/2`.  Ελέγχουμε αν οι παράμετροι που δεχόμαστε είναι δυαδικές ή όχι.  Αυτός είναι ο τρόπος να ελέγξουμε αν η παράμετρος είναι αλφαριθμητικό στην Elixir.

Μετά από αυτό, καλούμε μια συνάρτηση η οποία ταξινομεί τα αλφαριθμητικά σε αλφαβητική σειρά, πρώτα κάνοντας το αλφαριθμητικό σε μικρά και μετά χρησιμοποιώντας την `String.graphemes`, η οποία επιστρέφει μια λιστα με τα Γραφήματα του αλφαριθμητικού.  Πολύ εύκολο, έτσι;

Ας δούμε την έξοδο στο iex:

```elixir
iex> Anagram.anagrams?("Γειά", "άιεγ")
true

iex> Anagram.anagrams?("Μαρία", "ίΜαρα")
true

iex> Anagram.anagrams?(3, 5)
** (FunctionClauseError) no function clause matching in Anagram.anagrams?/2
    iex:2: Anagram.anagrams?(3, 5)
```

Όπως βλέπετε, η τελευταία κλήση στην `anagrams?` προξένησε ένα FunctionClauseError.  Αυτό το σφάλμα μας λέει ότι δεν υπάρχει συνάρτηση στην ενότητά μας με το πρότυπο δύο μη-δυαδικών παραμέτρων, και αυτό είναι ακριβώς που θέλουμε, να δεχτούμε δύο αλφαριθμητικά και τίποτα άλλο.