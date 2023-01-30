## Dataset Description
The dataset presented here is drawn from the Kolibri Studio curricular alignment tool, in which users can create their own **channel**, then build out a **topic tree** that represents a curriculum taxonomy or other hierarchical structure, and finally organize **content items** into these topics, by uploading their own content and/or importing existing materials from the Kolibri Content Library of Open Educational Resources.

An example of a branch of a topic tree is: `Secondary Education >> Ordinary Level >> Mathematics >> Further Learning >> Activities >> Trigonometry`. The leaf topic in this branch might then contain (be **correlated** with) a content item such as a video entitled `Polar Coordinates`.

You are challenged to predict which content items are best aligned to a given topic in a topic tree, with the goal of matching the selections made by curricular experts and other users of the Kolibri Studio platform. In other words, your goal is to recommend content items to curators for potential inclusion in a topic, to reduce the time they spend searching for and discovering relevant content to consider including in each topic.

Please note that this is a [Code Competition](https://www.kaggle.com/competitions/learning-equality-curriculum-recommendations/overview/code-requirements), in which the actual test set is hidden. In this public version, we give some sample data drawn from the training set to help you author your solutions. When your submission is scored, this example test data will be replaced with the full test set.

The full test set includes an additional 10,000 topics (none present in the training set) and a large number of additional content items. The additional content items are only correlated to test set topics.

## Files and Fields
The training set consists of a corpus of topic trees from within the Kolibri Content Library, along with additional non-public aligned channels, and supplementary channels with less granular or lower-quality alignment.

- **topics.csv** - Contains a row for each topic in the dataset. These topics are organized into "channels", with each channel containing a single "topic tree" (which can be traversed through the "parent" reference). Note that the hidden dataset used for scoring contains additional topics not in the public version. **You should only submit predictions for those topics listed in sample_submission.csv.**
  - `id` - A unique identifier for this topic.
  - `title` - Title text for this topic.
  - `description` - Description text (may be empty)
  - `channel` - The channel (that is, topic tree) this topic is part of.
  - `category` - Describes the origin of the topic.
    - `source` - Structure was given by original content creator (e.g. the topic tree as imported from Khan Academy). There are no topics in the test set with this category.
    - `aligned` - Structure is from a national curriculum or other target taxonomy, with content aligned from multiple sources.
    - `supplemental` - This is a channel that has to some extent been aligned, but without the same level of granularity or fidelity as an aligned channel.

  - `language` - Language code for the topic. May not always match apparent language of its title or description, but will always match the language of any associated content items.
  - `parent` - The id of the topic that contains this topic, if any. This field if empty if the topic is the root node for its channel.
  - `level` - The depth of this topic within its topic tree. Level 0 means it is a root node (and hence its title is the title of the channel).
  - `has_content` - Whether there are content items correlated with this topic. Most content is correlated with leaf topics, but some non-leaf topics also have content correlations.

- **content.csv** - Contains a row for each content item in the dataset. Note that the hidden dataset used for scoring contains additional content items not in the public version. These additional content items are only correlated to topics in the test set. Some content items may not be correlated with any topic.
  - `id` - A unique identifier for this content item.
  - `title` - Title text for this content item.
  - `description` - Description text. May be empty.
  - `language` - Language code representing the language of this content item.
  - `kind` - Describes what format of content this item represents, as one of:
    - `document` (text is extracted from a PDF or EPUB file)
    - `video` (text is extracted from the subtitle file, if available)
    - `exercise` (text is extracted from questions/answers)
    - `audio` (no text)
    - `html5` (text is extracted from HTML source)
  - `text` - Extracted text content, if available and if licensing permitted (around half of content items have text content).
  - `copyright_holder` - If text was extracted from the content, indicates the owner of the copyright for that content. Blank for all test set items.
  - `license` - If text was extracted from the content, the license under which that content was made available. Blank for all test set items.

- **correlations.csv** The content items associated to topics in the training set. A single content item may be associated to more than one topic. In each row, we give a `topic_id` and a list of all associated `content_ids`. These comprise the targets of the training set.

- **sample_submission.csv** - A submission file in the correct format. See the [Evaluation](https://www.kaggle.com/competitions/learning-equality-curriculum-recommendations/overview/evaluation) page for more details. **You must use this file to identify which topics in the test set require predictions.**
