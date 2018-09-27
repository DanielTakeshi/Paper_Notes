# Visual Measurement of Suture Strain for Robotic Surgery

A paper which addresses the lack of appropriate feedback to surgeons when
teleoperating robot surgical systems such as the da vinci.

"tactile" = relating to the sense of touch.

See: https://www.quora.com/Robotics-What-is-the-difference-between-tactile-feedback-and-haptic-feedback

Main contribution:

> Displaying suture strain in real time has potential to decrease the learning
> curve and improve the performance and safety of robotic surgical procedures.
> Conventional strain measurement methods involve installation of complex
> sensors on the robotic instruments. This paper presents a noninvasive video
> processing-based method to determine strain in surgical sutures. The method
> accurately calculates strain in suture by processing video from the existing
> surgical camera, making implementation uncomplicated.

I see, a video-based (or vision-based, I guess) system for providing additional
feedback to the surgeon. Similar to the "Sensory Substitution" paper from
(Okamura et al, 2005). But no Deep Learning. Note: the da vinci, they claim, has
*no force feedback*, which would explain the research literature here in the
first place!

Part of the motivation for doing this is because it is important to maintain
tension during suturing (as, again, I know) and this is not easily done by
simply looking at videos/images, some additional feedback is helpful, which is
what this paper addresses.

They cite the "Sensor Substitution" paper [6] as follows:

> It has been demonstrated that haptic feedback via visual and auditory cues
> does decrease the time to perform suture tying with robotic systems [6].

But rather than install complicated systems, they propose a non-invasive
video-based solution. Good! Well it depends on a lot of assumptions and is quite
hand-tuned, so unclear how this can generalize.
