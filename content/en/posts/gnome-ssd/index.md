+++
title = "So, why *should* GNOME support server side decorations?"
date = "2022-01-25"
lastmod = "2025-12-26"
author = "Blisterexe"
goimportpackage = true
cover = "cover.png"
+++

*Cet article est aussi disponible en [français](/fr/posts/gnome-ssd/).* 

This article contains quite a few technical terms, which I will explain these in the following paragraphs, those that are already familiar with these terms may skip to the next section. A basic understanding of linux and it's desktop environments is assumed.

Server side decorations (SSD) is the term for when when the application's titlebar is drawn by the system and client side decorations (CSD) is the term for when the applications draws it's own titlebar. KDE prefers the former, while GNOME prefers the latter. KDE and most other desktop environments supports both, while GNOME only supports CSD.

Since SSD are drawn by the desktop, instead of by the application, apps that use it have a seperate titlebar (bottom), while apps that are designed around CSD tend to have an integrated titlebar (top).

{{< figure src="csd-vs-ssd.png" alt="seperate titlebar (bottom) vs integrated titlebar (top)" position="center" caption="seperate titlebar (bottom) vs integrated titlebar (top)" captionPosition="center" >}}
*Image credit: Tobias Bernard*

I will refer to the mutter compositor as GNOME, compositors in general as "the system" and gtk4/libadwaita apps as GNOME apps, for simplicity's sake.

Without further ado, let's get to the titular question.

## Why *should* GNOME support SSD?

Well, for a few reasons:

#### Many apps handle CSD poorly.

Certain applications work poorly without SSD. These apps end up having either no titlebar or a weird one, and the user can't move or resize them easily. Davinci Resolve is one notable example, but it's not the only one.

GNOME is a large enough part of the linux desktop that most applications designed for SSD decorations implement a workaround in order to not be broken on GNOME. Usually, that workaround is [libdecor](https://github.com/neonkore/libdecor).

This sounds fine enough, except libdecor is the *worst* of both worlds because you get the space inefficiency and lack of integration of a traditional titlebar, with all the inconsistency and lack of user customization of an integrated titlebar.

{{< figure src="libdecor-inconsistency.png" alt="Three applications with visually distinct libdecor-provided titlebars" position="center" caption="Three applications with visually distinct libdecor-provided titlebars" captionPosition="center" >}}

This is not the fault of the libdecor devs, it's just because it's a bandaid fix for a wider issue.

Supporting SSD would solve this, as every application that does not wish to use CSD would have a nice native titlebar with consistent shading, colors, and corner radius, and developers would have to deal with [less work and frustration](https://factorio.com/blog/post/fff-408).

#### Supporting SSD would make apps "Feel more native"

On windows and macOS, any application can implement CSD in their application and have it respect the spacing, position and look of the titlebar buttons that the SSD equivalents that those platforms provide. 

The issue with doing that on linux is that the design of the titlebar can vary so wildy depending on the environment that having CSD's that "feel native" is a bit of a losing battle. Offering the option of SSD for non-GNOME apps on GNOME and vice versa would go a long way to making applications feel more native for users that value that sort of thing.

#### Many users of other desktops want their SSD respected.

A lot of users on other desktops are frustrated by GNOME apps not supporting SSD, breaking if SSD is force-enabled.

While an app breaking when users do something unsupported is obviously not the developer's fault, GNOME apps optionally supporting SSD would make many users of other desktops very happy indeed.

If you're wondering why a user would want SSD applied to a GNOME app, some people value titlebar consistency over the design of any individual app. [Some users](https://discuss.kde.org/t/i-believe-that-server-side-decorations-are-an-accessibility-feature-which-we-are-on-the-verge-of-losing/9529) view SSD as an accessibility feature as well.

#### GNOME's lack of support hurts the linux desktop.

Linux is already a small market, and GNOME not supporting the de facto standard that is `xdg-decoration` only hurts the linux desktop by increasing fragmentation.

This fragmentation only makes the linux desktop less attractive to developers and, importantly, makes application developers **less likely to adopt more modern standards such as wayland**.

## The arguments against SSD support

Many discussions about why GNOME should implement SSD stop at the arguments for it, but obviously there are also arguments against supporting SSD, otherwise GNOME would have implemented them by now.

Arguments such as:

#### "SSD is out of spec"

This is the main argument the GNOME developers use to justify why they don't support SSD. This is true, `xdg-decoration` is an "unstable" protocol, and wayland was originally designed with only CSD in mind. However, wayland was also initally designed without screenshare or global keybinds, standards that GNOME had since adopted.

The standard doesn't pose the same kind of security or design issues that standards like the system tray one do, either.

The fact is that it's adopted by [every production-ready desktop compositor](https://wayland.app/protocols/xdg-decoration-unstable-v1) other than GNOME's mutter, and is relied upon by application and desktop developers alike, making it a de facto standard that is widely adopted.

That is to say, while `xdg-decoration` is technically out of spec, the only thing distinguishing it from any wayland protocol that is "in spec" is GNOME's lack of support for it.

#### "Window decorations are part of the app, and thus shouldn't be the purview of desktop."

Many developers and users see the titlebar as something that is a part of the desktop, while some others disagree. This isn't a problem, just a difference in philosophy.

The real problem is the idea that GNOME project shouldn't cater to the first group. It would be like GNOME not supporting `xdg-file-chooser` and saying that each app *should* ship their own file picker. But GNOME does support it, and only apps that wish to implement their own file picker do so.

Since both approaches are used, and liked, miscellaneous advantages and disadvantages of either approach are irrelevant, and so are other arguments pertaining to design. This is why I haven't brought them up.

## Solution

Hopefully I have made my point as to why it would be worthwhile for GNOME to adopt server side decorations, but a question still remains.

#### What would it look like?

I have touched on what an implementation of SSD would look like a few times throughout this article without actually fully explaining what it would look like. So.

Obviously GNOME would implement the relevant protocols and enable server side decorations on all application that don't explicitely request SSD. This is in order to have SSD applied to apps that in only provide CSD as a fallback for compositors that don't support SSD.

To be clear, this *would not* apply to apps that are designed for a united headerbar, like GNOME apps.

On other desktops, however, GNOME apps would still request to not have SSD applied, a preference that is respected by every mainstream desktop. The only difference is that they would allow users the option to force a titlebar on GNOME apps. In this scenario, the GNOME app would remove the integrated window title and controls.

This would be the best of both worlds, since GNOME users would enjoy more consistent titlebars and window shadows on third party apps and **users** on other desktops would gain the ability to have SSD on GNOME apps if they so wish.

GNOME's lack of support for server side decorations is the single biggest issue with the GNOME desktop environment right now, in my opinion, so I sincerely hope this article can serve as a push to get it implemented.

If you want to help me with this, you can **share this article**.   
