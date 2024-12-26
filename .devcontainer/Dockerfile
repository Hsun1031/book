FROM mcr.microsoft.com/devcontainers/rust:1-1-bullseye

ARG USER vscode
USER ${USER}

# install mdbook
RUN cargo install mdbook
# or use the git
# RUN cargo install --git https://github.com/rust-lang/mdBook.git mdbook

# Add Path to .bashrc
RUN echo "export PATH=$PATH:/home/${USER}/.cargo/bin" >> /home/${USER}/.bashrc

CMD [ "/bin/bash" ]