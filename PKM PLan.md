## Overall Structure

1. Use PARA: Projects, Area, Resources, Archives.
2. Use Zettelkasten to create links between notes. Create a link whenever you think a connection can be made to another topic or if something is likely to need more clarity in a different note.
3. Create Indexes or MOCs to cover a topic in an area.
   i.e. if the area is programming, a topic/index is Rust.
4. Use metadata to organise notes and make templates for all kinds of notes.
5. Use tags as markers of context on the note, they should be additive not repetitive. i.e. if the note is "Rust - Ownership" the tags could be something like "resource management" or "RAII".
6. Tags can also be nested such as "programming/rust" giving more granularity.
7. Since tags should be used to denote context and not location, both in the knowledge base and in the topic they will not be helpful to query location, rather they are there to create connections across indices to find connections across disciplines.
8. To find the location of a file use the path metadata. The path should be the shortest route from the Home note down the indices into the note itself. The path should also include references to which folder of PARA the note finds itself in.

## Note Structure


Each template for every kind of note should include these metadata fields in this order:

- `aliases`: shorthand aliases for this note for easier linking
- `tags`: tags on the context related in the note
- `path`: path to this note from home, 
- `status`: how complete the note is
- `source`: the source of the information (url, book, podcast)
- `created`: the date the note was created

The `created` and `status` fields will be filled in automatically.
The rest will need to filled in manually each time. Some notes do not require all these fields and other may require more, but for now I think this is a good start.

Metadata should be focused on capturing information on the note that will create value when queried: 

- `created`: work over time
- `status`: progress/completion rate
- `path`: navigation within knowledge base
- `tags`: high level context on the topic in the note
- `source`: easy reference to original material
- `aliases`: easier reference to this note, increases fuzziness

`status` should reflect how complete a note is. The status reflects how much work is left to be done on a note until it is considered complete. The stages/values are:

- `Night`: For empty or bare bone notes. What Luhmann called Fleeting notes. Until handwritten notes or scribbles are added to the knowledge base they are considered `Night` notes.
- `Dawn`: These notes have been given a first pass. The writing in this note is mostly unoriginal, containing what was considered most valuable from the resources you're pulling from.
- `Noon`: This note has had a second pass through, where words have been rewritten to fit your own understanding of the topic.
- `Dusk`: This note is complete. It includes all the formatting such as footnotes, tags, links and references. This note can be set aside and considered fully digested. Luhmann would call this a Permanent Note.

`source` should contain a link to the original material that leads outside the knowledge AND a link to the Literature note if it exists.

## Style Guide

For consistency here are some rules for styling the notes as they are being written.

1. `backticks` should be used whenever a reference to a technical word is used, like the metadata fields above, or something like a keyword in a programming language is used; i.e. `const`, `await`, `use`. This is to help visually differentiate the words that are *of* the topic and words *about* the topic.
   
2. When creating a list under a paragraph block use a `:` and then leave an empty line between them:

   - like this.
   - it makes the overall block less cramped while the `:` denotes the connection between them.
   - Each line in a list should also end with a `.`.

3. When using units after a measurement do NOT leave a space between the value and the unit: 300g.
   
4. Create links often and quickly, you can always review later and tidy up duplicates or connect to an existing note. The creation of links should work like a record of your interest without detracting focus from the current topic. When I am writing my note and think "oh I need to know more about that" or "that's cool I should check it out", create the link around the relevant words and move on. These empty links can be a great place to start on new but related avenues of thought when stuck on a topic.
   
5. When writing lists where each entry is a more than one or two lines, keep and empty line between each entry to maintain readability. Otherwise the blocks get too crowded.

6. If the list is full of short one lines however, keep them close together.

7. Hold `shift` when entering a new line to keep it in line with your indented block.

## Note Taking 

In the case of Literature notes, where you're not trying to translate the author's understanding into your own these steps are simplified. You still pass through all four but the first pass might include everything thing you think interesting from the source, while the second trims it down to its most relevant pieces.The polishing required here is minimal, just a reference to the original and the appropriate metadata.
