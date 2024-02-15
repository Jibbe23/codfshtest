# Verwenden Sie das offizielle Node.js-Image als Basis
FROM node:latest

# Installieren Sie notwendige Pakete für die Installation und das Skript
RUN apt-get update && apt-get install -y \
    curl \
    git \
    build-essential \
    zlib1g-dev \
    ruby-full \
    default-jre \
    wget \
    apt-transport-https \
    lsb-release \
    gnupg \
    && rm -rf /var/lib/apt/lists/*

# Hinzufügen des Microsoft-Paketrepository (optional, falls weitere Microsoft-Pakete benötigt werden)
RUN wget -q https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && apt-get update \
    && rm -rf /var/lib/apt/lists/* /packages-microsoft-prod.deb

# Installieren Sie das .NET SDK 6.0 manuell
RUN curl -L https://dot.net/v1/dotnet-install.sh > dotnet-install.sh \
    && chmod +x dotnet-install.sh \
    && ./dotnet-install.sh --channel 6.0 --install-dir /usr/share/dotnet \
    && rm dotnet-install.sh

# Fügen Sie das .NET SDK zum PATH hinzu, falls es nicht automatisch geschieht
ENV PATH="/usr/share/dotnet:${PATH}"

# Setzen Sie Ihr Arbeitsverzeichnis
WORKDIR /app

# Optional: Kopieren Sie Ihre Anwendungsdateien in das Image
# COPY . /app

# Set up Ruby Gems
RUN echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc \
    && echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc \
    && gem install jekyll bundler

# Install SUSHI
RUN npm install -g fsh-sushi

# Install Firely Terminal
RUN dotnet tool install --global firely.terminal

# Download HAPI Validator
RUN wget https://github.com/hapifhir/org.hl7.fhir.core/releases/latest/download/validator_cli.jar -O /usr/local/bin/validator_cli.jar

# Bereinigen Sie unnötige Dateien
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Fügen Sie den globalen Tools-Pfad hinzu
ENV PATH="${PATH}:/root/.dotnet/tools"

# Führen Sie einen Befehl aus, um zu überprüfen, ob die Installation erfolgreich war
CMD [ "dotnet", "--version" ]
