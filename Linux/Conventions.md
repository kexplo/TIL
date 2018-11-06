# .dist file

from: https://stackoverflow.com/a/16843246/1545387

`.dist` files are often configuration files which do not contain the real-world deploy-specific parameters (e.g. Database Passwords, etc.), and are there to help you get started with the application/framework faster. So, to get started with such frameworks, you should remove the `.dist` extension, and customize your configuration file with your personal parameters.

One purpose I have seen in using `.dist` extension, is to avoid publishing personal data on VCSs (say git). So, you, as the developer of a reusable app, would use your own configuration file, but put the de-facto get-started config data in a separate `.dist`-suffixed file.
