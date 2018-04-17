# Learning to Push by Grasping: Using Multiple Tasks for Effective Learning

To help reduce data-hungry nature of self-supervision tasks, they propose to
share among tasks. They show that by combining pushing and grasping data, they
can get better performance than either alone (even if you allocate 2x more data
to each individual task to make the data comparable). They argue this is due to
perhaps better regularization and exposure to a wider variety of data.

This isn't quite the same as fine-tuning, FYI: it's joint training of the tasks
both from scratch.

The tasks here: pushing, grasping, and poking. The neural net is pretty obvious,
shared convolutional layers followed by task-specific layers.

The conclusion:

> This paper attempts to break the myth of task-specific learning and shows that
> multi-task learning is not only effective but in fact improves the performance
> even when the total amount of data is the same. We hypothesize this is
> primarily because of diversity of data and regularization in learning. This
> paper opens up a new subfield of multi-task learning in robotics; specifically
> focusing on mechanism of sharing across different tasks.

BTW: how is this a myth? It seems obvious to me that data can be shared ... but
maybe I don't have the pre-2017 perspective?
