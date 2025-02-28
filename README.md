# cl-completions
> A Common Lisp LLM completions library

Usage
------

`cl-completions` is available via [ocicl](https://github.com/ocicl/ocicl).  Install it like so:
```
$ ocicl install completions
```

`cl-completions` supports [ollama](https://ollama.com/), [OpenAI](https://openai.com/blog/openai-api), and [Anthropic](https://anthropic.com/api) APIs.

To use the ollama API:

```
(let ((completer (make-instance 'ollama-completer :model "mistral:latest")))
  (get-completion completer "It's a beautiful day for " 100))
```

To use the OpenAI API:

```
(let ((completer (make-instance 'openai-completer :api-key (uiop:getenv "OPENAI_API_KEY"))))
  (get-completion completer "It's a beautiful day for " 100))
```

To use the Anthropic API:

```
(let ((completer (make-instance 'anthropic-completer :api-key (uiop:getenv "ANTHROPIC_API_KEY"))))
  (get-completion completer "It's a beautiful day for " 100))
```

In addition, the OpenAI completer supports callback functions, like so:

```
(defun-tool time-of-day ()
  "Useful if you want to know what time it is."
  (let ((decoded-time (multiple-value-list (get-decoded-time))))
    (format nil "Current time: ~2,'0D:~2,'0D:~2,'0D~%"
            (third decoded-time)
            (second decoded-time)
            (first decoded-time))))

(defun-tool get-temperature ((location string "Where to get the temperature for."))
  "Get the temperature for a specific location"
  (cond
    ((equal location "Toronto")
     "cold")
    (t "warm")))

(let ((c (make-instance 'openai-completer
                        :api-key (uiop:getenv "OPENAI_API_KEY")
                        :tools '(time-of-day get-temperature))))
  (get-completion c "I'm in Toronto. What's the time and temperature here?" 20))
```

This code generates output like:
```
The current time in Toronto is 20:01:22 and it's cold there right now
```

Related Projects
-----------------

Related projects include:
* [cl-text-splitter](https://github.com/atgreen/cl-text-splitter): a text splitting library
* [cl-embeddings](https://github.com/atgreen/cl-embeddings): an LLM embeddings library
* [cl-chroma](https://github.com/atgreen/cl-chroma): for a Lisp interface to the [Chroma](https://www.trychroma.com/) vector database.
* [cl-chat](https://github.com/atgreen/cl-chat): a wrapper around `completions` to maintain chat history,

Author and License
-------------------

``cl-completions`` was written by [Anthony
Green](https://github.com/atgreen) and is distributed under the terms
of the MIT license.
