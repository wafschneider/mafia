# mafia - Modular Application for FOLIO: Installation Archive

This is a project to put FOLIO's foundational modularity to work, enabling a FOLIO installation in practice to support modular use of applications provided by third parties rather than being limited to the modules provided as part of a flower release. Getting this to happen involves both technical and social/political work. This project represents work on the technical aspect.


## Manifesto

Quoting from the abstract of _Modularity in FOLIO: principles, techniques and tools_ ([available as a preprint from Zenodo](https://zenodo.org/record/5703010): and submitted to the forthcoming FOLIO-themed special issue of the _International Journal of Librarianship_):

> From its earliest inception, FOLIO was conceived not as an ILS (Integrated Library System), but as a true Services Platform, composed of many independent but interdependent modules, and forming a foundation on which an ILS or other library software could be built out of relevant modules. This vision of modularity is crucial to FOLIO’s appeal to the library community, because it lowers the bar to participation: individual libraries may create modules that meet their needs, or hire developers to do so, or contribute to funding modules that will be of use to a broader community — all without needing “permission” from a central authority.

But at the time of writing, organizational issues mean that it is not straightforward in practice for organizations running FOLIO to install modules provided by sources other than their vendor, most often as a part of a ["flower release"](https://wiki.folio.org/display/REL/Flower+Release+Names). Module inclusion in flower releases is governed by [the FOLIO Technical Council](https://wiki.folio.org/display/TC/Technical+Council), whose remit is to emphasize maintaining quality in a curated set of modules. While this emphasis in some respects serves the community well, it does have the unwelcome side-effect of discouraging the creation of experimental modules, or making experimental deployments of works in progress.

The present project aims to remove technical barriers to easier packaging and deployment of FOLIO applications. In doing so, our hope is to make it easier for software developers to deliver FOLIO applications, and for system administrators to install those applications.


## Areas of work

Setting up and running a FOLIO instance entails three quite separate areas of work.

1. Obtaining and running a minimal base system, including Okapi and Stripes.

2. Obtaining and installing all the modules that will be needed for any tenant to run on the installation.

3. For each tenant, selecting which modules are to be included in that tenant's setup.

(In setting up a Stripes development platform, task 2 is captured by the platform's `package.json`, which specifies which front-end packages to install -- and, by implication, which back-end modules ought to be installed to support them. And task 3 is captured by the `stripes.config.js`, which specifies (among other things) which UI modules should be included in the bundle built for a specific tenant. In principle, there should be many `stripes.config.js` files for each platform -- one for each tenant -- but because development systems tend to run a single tenant, the distinction between the purposes of these two files is often overlooked.)

Improvements are there to be made in all three of these areas:

1. At present, a "minimal" FOLIO system is uncomfortably large, due to a maze of dependencies that should be optional but are not. For example, the login process involves service-points, a concept that ties together users and inventory and so requires both user-mamanagement and inventory modules to be pull into the system. Work is under way to fix at least some of these problems ([FOLIO-3253](https://issues.folio.org/browse/FOLIO-3253), [stripes-core pull-request 1101](https://github.com/folio-org/stripes-core/pull/1101)), so that a minimal FOLIO becomes small enough to function as a sensible platform for non-ILS application suites.

2. A FOLIO application is typically composed of multiple modules, some on the front-end and some on the back-end. At present, there is no good way to install all the components of an application in one shot. This means there is in some sense no such _thing_ as an application -- only aggregates of modules -- and therefore no simple was to install an application.

3. While Okapi provides [extensive and well-documented WSAPIs](https://github.com/folio-org/okapi/blob/master/doc/guide.md#okapis-own-web-services) for managing which modules are available to a tenant, there is no UI to these APIs, meaning that maintaining tenants is a job requiring devops skills.


## Practical steps

There are several technical and social facilitiesb that would need to be provided for a fully general FOLIO ecosysem to come into being. These include but may not be limited to the following:

* Addressing task 1 above, a FOLIO-app "package" format -- a single concrete thing, broadly analogous to a Debian package, that a vendor can create and deliver, and that a FOLIO implementor can install. This may end up simply being a structured metadata file that points to other release-artifacts packages and includes information about how to deploy them. See [_Thoughts on the MAFIA package format_](doc/package.md),
* Some notion of module certificaton and assessment, so FOLIO implementors who want to install an app can have a degree of confidence in it. This will likely entail cryptographic signing, but also perhaps some form of independent QA, reviews, scores, etc.
* Addressing task 2 avove, a simple UI to basic Okapi operations such as enabling and disabling modules. This might initially just be a page in [`ui-developer`](https://github.com/folio-org/ui-developer/), and might eventually develop in to an "app store" that can be used to install packages.


## Related work

* Mike Gorrell's draft document [_OLF Release/Package/App Store_](https://docs.google.com/document/d/1eaCwFLydFIviMiVrDBhJmTO_wuRgAMNsK2QEmQqMcZU/edit?pli=1#heading=h.7430ujqzdamg).


