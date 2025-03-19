# Groot N1
### Was sind die wichtigsten architektonellen Innovationen von GR00T N1 und wie tragen sie zur Vielseitigkeit bei?
Die wichtigsten architektonellen Innovationen von GR00T N1 sind sein **duales System Design** als Vision-Language-Action (VLA) Modell und seine Fähigkeit zur **Unterstützung verschiedener Roboter-Bauformen**. Diese Innovationen tragen maßgeblich zu seiner Vielseitigkeit bei.

Im Detail sind die architektonischen Kernpunkte von GR00T N1:

*   **Duales System Architektur:** GR00T N1 verwendet eine **kompositionelle Architektur**, die von der menschlichen kognitiven Verarbeitung inspiriert ist. Sie besteht aus zwei Hauptmodulen:
    *   **System 2: Vision-Language Modul (VLM):** Dieses Modul, basierend auf dem **NVIDIA Eagle-2 VLM**, interpretiert die Umgebung durch visuelle Wahrnehmung und Sprachinstruktionen. Es arbeitet mit einer Frequenz von 10Hz auf einer NVIDIA L40 GPU. Das VLM verarbeitet Bild- und Text-Token, um das Umfeld zu verstehen und das Aufgabenziel zu erfassen. Für das öffentlich zugängliche GR00T-N1-2B Modell werden die Repräsentationen der 12. Schicht des LLMs verwendet, was zu einer schnelleren Inferenz und einer höheren Erfolgsrate bei nachgeschalteten Aufgaben führt.
    *   **System 1: Diffusions-Transformer Modul:** Dieses Modul generiert **flüssige motorische Aktionen in Echtzeit** mit einer höheren Frequenz von 120Hz. Es verwendet einen Diffusions-Transformer, der mit Action Flow Matching trainiert wurde. Dieses Modul greift über Cross-Attention auf die Output-Token des VLMs zu und verwendet **bauformspezifische Encoder und Decoder**, um variable Zustands- und Aktionsdimensionen für die Bewegungserzeugung zu handhaben. Die Aktionen werden in Chunks verarbeitet, wobei in der Implementierung eine Chunk-Größe von 16 verwendet wird.

*   **Cross-Embodiment Unterstützung:** GR00T N1 ist so konzipiert, dass es **verschiedene Roboter-Bauformen unterstützt**, von Tischroboterarmen bis hin zu humanoiden Robotern mit komplexen Händen. Um die unterschiedlichen Zustandsbeobachtungen und Aktionen verschiedener Roboter-Bauformen zu verarbeiten, werden **bauformspezifische MLPs (State und Action Encoder)** verwendet, die diese in einen gemeinsamen Embedding-Raum projizieren, bevor sie in den Diffusions-Transformer (DiT) eingegeben werden. Am Ende des DiT befindet sich ein weiterer bauformspezifischer MLP (Action Decoder), um die finalen Aktionsbefehle für den jeweiligen Roboter vorherzusagen. Diese Module unterstützen auch latente Aktionen, die aus Humanvideos oder inversen Dynamikmodellen (IDM) abgeleitet werden können, was die Trainingsdatenbasis erheblich erweitert.

Wie tragen diese Innovationen zur Vielseitigkeit bei?

*   Die **duale Systemarchitektur** ermöglicht es GR00T N1, sowohl über die Aufgabe nachzudenken und sie zu verstehen (System 2) als auch die notwendigen motorischen Fähigkeiten zur Ausführung zu generieren (System 1). Die enge Kopplung und das gemeinsame end-to-end Training beider Module fördern die Koordination zwischen Planung und Ausführung, was für eine breite Palette von Aufgaben in verschiedenen Umgebungen entscheidend ist.
*   Die **Fähigkeit zur Cross-Embodiment Unterstützung** bedeutet, dass das gleiche trainierte Modell auf verschiedenen Robotertypen eingesetzt werden kann, ohne dass für jede Bauform ein separates Modell trainiert werden muss. Dies wird durch die bauformspezifischen Encoder und Decoder ermöglicht, die die unterschiedlichen sensorischen und motorischen Fähigkeiten der Roboter abstrahieren und so eine gemeinsame Verarbeitung im Kernmodell erlauben. Die Nutzung latenter Aktionen aus Humanvideos erweitert zudem die Anwendbarkeit des Modells auf Szenarien, die keine direkten Roboterdaten beinhalten.
*   Die Verwendung eines **Vision-Language Modells (VLM)** als Reasoning-Modul erlaubt es GR00T N1, Aufgaben zu verstehen, die in natürlicher Sprache beschrieben sind, und visuelle Informationen aus der Umgebung zu nutzen, um diese Aufgaben auszuführen. Dies ermöglicht eine flexible Interaktion und die Bewältigung neuartiger Situationen.
*   Der **Diffusions-Transformer mit Action Flow Matching** ermöglicht die Generierung flüssiger und robuster Bewegungen. Die iterative Denoising-Methode führt zu präzisen Aktionen, die für eine Vielzahl von Manipulationsaufgaben erforderlich sind.

Zusammenfassend lässt sich sagen, dass die duale Systemarchitektur, die Cross-Embodiment Unterstützung durch bauformspezifische Module und die Nutzung eines VLM in Kombination mit einem Diffusions-Transformer GR00T N1 zu einem vielseitigen Fundamentmodell für humanoide Roboter machen, das in der Lage ist, ein breites Spektrum an Aufgaben auf verschiedenen Robotertypen zu erlernen und auszuführen.

### Beschreiben Sie die Dual-System-Architektur von GR00T N1 kurz.
Die **Dual-System-Architektur** von GR00T N1 ist ein Kern seiner Konzeption als Vision-Language-Action (VLA) Modell für humanoide Roboter. Sie besteht aus zwei Hauptmodulen, die eng miteinander gekoppelt und gemeinsam trainiert werden:

*   **System 2: Vision-Language Modul (VLM)**: Dieses Modul, basierend auf dem **NVIDIA Eagle-2 VLM**, dient als **Reasoning-Modul**. Es interpretiert die Umgebung durch **visuelle Wahrnehmung** (Bilder) und **Sprachinstruktionen**. Es verarbeitet diese Eingaben, um das **Aufgabenziel zu verstehen**. Das VLM läuft mit 10Hz auf einer NVIDIA L40 GPU. Für das GR00T-N1-2B Modell werden die **mittleren Schichten des LLMs** für die Repräsentationen genutzt, was eine schnellere Inferenz und höhere Erfolgsraten ermöglicht.

*   **System 1: Diffusions-Transformer Modul**: Dieses Modul ist das **Aktionsmodul** und generiert **flüssige motorische Aktionen in Echtzeit** mit einer höheren Frequenz von 120Hz. Es basiert auf einem **Diffusions-Transformer**, der mit **Action Flow Matching** trainiert wurde. System 1 empfängt die Output-Token des VLMs über **Cross-Attention** und nutzt **bauformspezifische Encoder und Decoder**, um variable Zustands- und Aktionsdimensionen für die **Bewegungserzeugung** zu handhaben.

Beide Systeme, System 1 und System 2, sind als **Transformer-basierte neuronale Netze** implementiert. Sie sind **eng gekoppelt und werden end-to-end gemeinsam optimiert**, um die Koordination zwischen **Reasoning und Ausführung (Aktion)** zu erleichtern. Das VLM (System 2) interpretiert die Aufgabe, und der Diffusions-Transformer (System 1) generiert die entsprechenden motorischen Aktionen, die zur Erfüllung dieser Aufgabe notwendig sind.



### Häufig gestellte Fragen zu GR00T N1
1. Was ist GR00T N1 und was macht es besonders?
GR00T N1 ist ein offenes Foundation Model für generalistische humanoide Roboter, das von NVIDIA entwickelt wurde. Es handelt sich um ein Vision-Language-Action (VLA)-Modell mit einer dualen Systemarchitektur, inspiriert von der menschlichen Kognition. System 2 interpretiert die Umgebung und Aufgaben durch Sehen und Sprache, während System 1, ein Diffusion Transformer Modul, flüssige motorische Aktionen in Echtzeit generiert. Beide Systeme sind eng gekoppelt und werden end-to-end auf einer vielfältigen Datenbasis trainiert, die reale Roboterdaten, menschliche Videos und synthetische Daten umfasst. GR00T N1 unterstützt verschiedene Roboterformen und zeigt hohe Leistung und Dateneffizienz bei komplexen Manipulationsaufgaben.

2. Wie ist die Architektur von GR00T N1 aufgebaut?
Die Architektur von GR00T N1 besteht aus zwei Hauptmodulen: einem Vision-Language Model (VLM) namens Eagle-2 (System 2) und einem Diffusion Transformer (DiT)-basierten Aktionsmodul (System 1). Das VLM verarbeitet visuelle und sprachliche Eingaben, um die Umgebung und Aufgaben zu verstehen. Der DiT nutzt diese Informationen zusammen mit dem Roboterzustand und generiert daraufhin motorische Aktionen durch einen iterativen Denoising-Prozess (Flow Matching). Embodiment-spezifische Encoder und Decoder ermöglichen die Anpassung an verschiedene Roboterformen. Beide Module sind Transformer-basiert und werden gemeinsam trainiert, um eine nahtlose Koordination zwischen Wahrnehmung, Kognition und Aktion zu gewährleisten.

3. Welche Arten von Daten werden zum Training von GR00T N1 verwendet und warum ist dieser Datenmix wichtig?
GR00T N1 wird auf einer heterogenen Datenbasis trainiert, die in einer Datenpyramide organisiert ist. Die Basis bilden große Mengen an Webdaten und menschlichen Videos, die breite visuelle und Verhaltensmuster liefern. Die mittlere Schicht besteht aus synthetischen Daten, die durch Simulationen und Videogenerierungsmodelle erzeugt werden und zur Augmentierung realer Daten dienen. Die Spitze der Pyramide bilden reale Roboterdaten, die spezifische, embodimentbezogene Informationen liefern. Dieser vielfältige Datenmix ist entscheidend, um das Problem der "Dateninseln" zu überwinden und ein generalistisches Modell zu trainieren, das in der Lage ist, in verschiedenen Situationen zu agieren und neue Aufgaben effizient zu lernen.

4. Wie werden Aktionsdaten aus Videos ohne direkte Roboteraktionen gewonnen?
Für menschliche Videos und synthetische, durch neuronale Netze generierte Trajektorien, bei denen keine direkten Roboteraktionsdaten vorliegen, verwendet GR00T N1 zwei Haupttechniken: Lernen latenter Aktionen (Latent Action Learning) und Inverse Dynamics Models (IDM). Beim Lernen latenter Aktionen wird ein VQ-VAE-Modell trainiert, um latente Aktionscodes aus aufeinanderfolgenden Videobildern zu extrahieren. Diese latenten Codes dienen dann als Trainingsziele. Beim Einsatz von IDMs wird ein Modell trainiert, um Pseudo-Aktionen basierend auf dem Übergang von einem Zustand zum nächsten (oft repräsentiert durch zwei aufeinanderfolgende Bilder) zu inferieren. Diese Techniken ermöglichen es, aktionslose Videodaten als zusätzliche "Embodiments" für das Modelltraining zu nutzen und so die Datengrundlage zu erweitern.

5. Welche Rolle spielen synthetische Daten und neuronale Trajektorien im Training von GR00T N1?
Synthetische Daten, einschließlich Simulationstrajektorien und neuronal generierter Trajektorien, spielen eine wichtige Rolle bei der Skalierung und Diversifizierung des Trainingsdatensatzes für GR00T N1. Simulationen ermöglichen die automatisierte Generierung großer Mengen an Daten in kontrollierten Umgebungen. Neuronale Trajektorien, die durch Feinabstimmung von Videogenerierungsmodellen auf realen Roboterdaten erzeugt werden, erlauben die Erstellung von "kontrafaktischen" Szenarien mit neuen Sprachprompts, wodurch die Menge und Vielfalt der Trainingsdaten erheblich gesteigert werden kann. Diese synthetischen Daten helfen, die Lücke der teuren und zeitaufwändigen realen Datenerfassung zu schließen und die Generalisierungsfähigkeit des Modells zu verbessern.

6. Wie wird GR00T N1 evaluiert und welche Ergebnisse wurden erzielt?
GR00T N1 wird in einer Vielzahl von simulierten und realen Benchmarks evaluiert, um seine Leistungsfähigkeit über verschiedene Roboterformen und Manipulationsaufgaben hinweg zu testen. In Simulationen zeigt GR00T N1 überlegene Ergebnisse im Vergleich zu State-of-the-Art Imitationslernverfahren auf verschiedenen Roboterkörpern. In realen Experimenten mit dem Fourier GR-1 humanoiden Roboter demonstriert das Modell eine starke Leistung bei sprachgesteuerten, beidhändigen Manipulationsaufgaben mit hoher Dateneffizienz. Insbesondere zeigt das vortrainierte Modell eine bemerkenswerte Fähigkeit zur Generalisierung auf neue Objekte und Situationen, selbst bei geringen Mengen an spezifischen Trainingsdaten für einzelne Aufgaben.

7. Welche Bedeutung hat die Offenheit von GR00T N1 und seiner Trainingsdaten?
Die Offenheit von GR00T N1, einschließlich der Veröffentlichung des 2B-Parameter-Modell-Checkpoints, der Trainingsdaten und der Simulationsumgebungen, ist von großer Bedeutung für die Robotik-Community. Sie ermöglicht es Forschern und Entwicklern, auf einem fortschrittlichen Foundation Model aufzubauen, Experimente zu reproduzieren, das Modell an ihre spezifischen Bedürfnisse anzupassen und zur Weiterentwicklung generalistischer humanoider Roboter beizutragen. Durch die Bereitstellung der Ressourcen möchte NVIDIA den Fortschritt in diesem Bereich beschleunigen und die Zusammenarbeit innerhalb der Community fördern.

8. Was sind die aktuellen Einschränkungen von GR00T N1 und welche zukünftigen Entwicklungen sind geplant?
Aktuell konzentriert sich GR00T N1 hauptsächlich auf kurzfristige Tischmanipulationsaufgaben. Zukünftige Arbeiten zielen darauf ab, die Fähigkeiten des Modells auf längerfristige Lokomotions- und Manipulationsaufgaben auszudehnen, was Fortschritte in der Hardware, der Modellarchitektur und den Trainingsdaten erfordert. Geplant sind Verbesserungen der Vision-Language Backbone, um das räumliche Denken und das Sprachverständnis zu verbessern. Weiterhin soll die Generierung synthetischer Daten verbessert werden, um vielfältigere und physikalisch plausiblere Daten zu erzeugen. Auch die Erforschung neuer Modellarchitekturen und Pre-Training-Strategien zur Verbesserung der Robustheit und Generalisierungsfähigkeit steht im Fokus zukünftiger Entwicklungen.

### Foundation Model für generalistische humanoide Roboter
Dieses Dokument fasst die wichtigsten Themen, Ideen und Fakten aus dem "GR00T N1 Whitepaper" zusammen. Der Fokus liegt auf der Vorstellung von GR00T N1, einem offenen Foundation Model für generalistische humanoide Roboter, seiner Architektur, den Trainingsdaten, der Evaluierung und den Schlussfolgerungen der Autoren.

Hauptthemen und wichtige Ideen/Fakten:

1. Einführung und Motivation:

Die Entwicklung autonomer Roboter für alltägliche Aufgaben in der menschlichen Welt ist ein bedeutendes technisches Ziel.
Fortschritte in Roboterhardware, künstlicher Intelligenz und beschleunigtem Rechnen haben die Entwicklung generalistischer Roboterautonomie ermöglicht.
Die Autoren plädieren für eine Full-Stack-Lösung, die Hardware, Modelle und Daten integriert, um menschliche körperliche Intelligenz zu erreichen.
Humanoide Roboter werden als überzeugende Hardwareplattform für Roboterintelligenz aufgrund ihrer menschenähnlichen Physis und Vielseitigkeit hervorgehoben.
Die Vielfalt und Variabilität der realen Welt erfordert generalistische Robotermodelle, die verschiedene Aufgaben bewältigen können.
Die Akquise von realen Humanoid-Daten in großem Maßstab ist kostspielig und zeitaufwendig, daher ist eine effektive Datenstrategie entscheidend.
Das Problem der "Dateninseln" wird angesprochen, bei dem fragmentierte Datensätze aufgrund unterschiedlicher Roboterverkörperungen, Sensoren und Kontrollmodi die Entwicklung eines wirklich generalistischen Modells behindern. "control modes, and other factors result in an archipelago of “data islands” rather than a coherent, Internet-scale dataset needed for training a true generalist model."
2. GR00T N1 Foundation Model:

Vorstellung: GR00T N1 ist ein offenes Foundation Model für generalistische humanoide Roboter. "We introduce GR00T N1, an open foundation model for generalist humanoid robots."
Architektur: Es handelt sich um ein Vision-Language-Action (VLA)-Modell mit einer Dual-System-Architektur, inspiriert durch menschliche kognitive Prozesse (Kahneman, 2011).
System 2 (Vision-Language-Modul): Interpretiert die Umgebung durch visuelle Wahrnehmung und Sprachinstruktionen. Es basiert auf einem vortrainierten Vision-Language Model (VLM) (NVIDIA Eagle-2 VLM). Es läuft mit 10Hz auf einer NVIDIA L40 GPU.
System 1 (Diffusion Transformer Modul): Generiert flüssige motorische Aktionen in Echtzeit (120Hz). Es verwendet einen Diffusion Transformer, der mit Action Flow Matching trainiert wurde. Es cross-attendiert die Output-Token des VLM und nutzt verkörperungsspezifische Encoder und Decoder.
Beide Module sind Transformer-basierte neuronale Netze, eng gekoppelt und end-to-end gemeinsam trainiert.
Cross-Embodiment-Support: GR00T N1 unterstützt verschiedene Roboterverkörperungen, von Tischroboterarmen bis hin zu humanoiden Robotern.
Datenpyramide: Das Training erfolgt mit einer heterogenen Mischung aus realen Robotertrajektorien, menschlichen Videos und synthetisch generierten Datensätzen, organisiert als Datenpyramide. "Rather than treating the training datasets as a homogeneous pool, we organize heterogeneous sources by scale: large quantities of web data and human videos lay the base of the pyramid; synthetic data generated with physics simulations and/or augmented by off-the-shelf neural models form the middle layer, and real-world data collected on the physical robot hardware complete the top."
Co-Training-Strategie: Eine effektive Co-Training-Strategie wird verwendet, um über die gesamte Datenpyramide hinweg zu lernen.
Für aktionslose Datenquellen (menschliche Videos) werden latente Aktionscodebücher und ein trainiertes inverses Dynamikmodell (IDM) verwendet, um Pseudo-Aktionen abzuleiten. "To train our model with action-less data sources, such as human videos and neural-generated videos, we learn a latent-action codebook (Ye et al., 2025) and also use a trained inverse dynamics model (IDM) to infer pseudo-actions."
Modellparameter: Das öffentlich freigegebene GR00T-N1-2B Modell hat 2,2 Milliarden Parameter, davon 1,34 Milliarden im VLM.
Inferenzzeit: Die Inferenzzeit für die Stichprobenentnahme eines 16-Aktions-Chunks beträgt 63,9 ms auf einer L40 GPU mit bf16.
3. Training Data Generation:

Datenpyramide (Wiederholung): Bestehend aus Webdaten und menschlichen Videos (Basis), synthetischen Daten (Mitte) und realen Roboterdaten (Spitze).
Latente Aktionen: Für menschliche Videos und neuronale Trajektorien werden latente Aktionen durch Training eines VQ-VAE-Modells extrahiert, um Features aus aufeinanderfolgenden Bildern zu lernen. Dies ermöglicht die Vereinheitlichung verschiedener Datenquellen in einem gemeinsamen latenten Aktionsraum. "For these data, we instead generate latent actions by training a VQ-VAE model to extract features from consecutive image frames from videos (Ye et al., 2025)."
Neuronale Trajektorien: Image-to-Video-Generierungsmodelle werden auf hausintern gesammelten Teleoperationsdaten feinjustiert, um die Datenmenge und -diversität zu erhöhen und kontrafaktische Szenarien zu generieren. "To harness these models, we fine-tune image-to-video generation models ... on all of our 88 hours of in-house collected teleoperation data and generate 827 hours of video data given the existing initial frames with novel language prompts, augmenting it by around 10×."
Simulationsdaten: Generiert mit Physiksimulatoren und einem DexMimicGen-basierten automatisierten Datengenerierungssystem. Der Fokus liegt auf "Rearrange A from B to C"-Aufgaben mit verschiedenen Objekten und Behältern.
Reale Roboterdaten: Umfassen Teleoperationsdaten, Open X-Embodiment-Datensätze (RT-1, Bridge-v2, Language Table, DROID, MUTEX, RoboSet, Plex) und AgiBot-Alpha.
4. Evaluierung:

Simulationsbenchmarks: RoboCasa Kitchen (24 Aufgaben, Franka Emika Panda), DexMimicGen Cross-Embodiment Suite (9 Aufgaben, verschiedene bimanuelle Roboter), GR-1 Tabletop Tasks (24 Aufgaben, GR-1 Humanoid).
Real-World-Experimente: Tabletop-Manipulationsaufgaben mit dem Fourier GR-1 Humanoid, um die Fähigkeit zu demonstrieren, neue Fähigkeiten aus wenigen menschlichen Demonstrationen zu erlernen.
Metriken: Erfolgsrate, teilweise Bewertungssystem für reale Roboter zur Erfassung des Verhaltens in verschiedenen Ausführungsphasen.
Ergebnisse:GR00T N1 übertrifft State-of-the-Art Imitationslern-Baselines in Simulationsbenchmarks.
Starke Leistung in realen Welt-Experimenten mit dem GR-1 Humanoid bei sprachbedingten bimanualen Manipulationsaufgaben mit hoher Dateneffizienz.
Vorgetrainiertes GR00T-N1-2B zeigt hohe Erfolgsraten bei der koordinierten bimanualen Manipulation und der Manipulation neuartiger Objekte in unbekannten Behältern. "GR00T-N1-2B achieves a success rate of 76.6% ... in the first coordinated setting and 73.3% ... in the second setting involving novel object manipulation."
In Low-Data-Regimen zeigt GR00T N1 signifikante Vorteile gegenüber Baselines wie Diffusion Policy.
5. Limitationen und zukünftige Arbeit:

Der aktuelle Fokus liegt auf kurzfristigen Tabletop-Manipulationsaufgaben.
Zukünftige Ziele umfassen die Erweiterung auf langfristige Lokomotion und Manipulation (Loco-Manipulation).
Verbesserungen des Vision-Language-Backbones zur Steigerung des räumlichen Denkens, des Sprachverständnisses und der Anpassungsfähigkeit sind geplant.
Die Weiterentwicklung synthetischer Datengenerierungstechniken zur Erzeugung diverserer und physikalisch plausibler Daten ist ein Schwerpunkt. "However, existing methods still face challenges in generating diverse and counterfactual data, while adhering to the laws of physics, limiting the quality and variability of synthetic datasets."
Erforschung neuer Modellarchitekturen und Pre-Training-Strategien zur Verbesserung der Robustheit und Generalisierungsfähigkeit.
6. Verwandte Arbeiten:

Diskussion über Foundation Models im Bereich der Robotik, einschließlich der Nutzung vortrainierter Modelle als High-Level-Reasoning-Module und der Feinabstimmung für Vision-Language-Action (VLA)-Modelle.
Hervorhebung der Einzigartigkeit von GR00T N1 durch die Verwendung großer synthetischer Simulationsdatensätze (MimicGen, DexMimicGen) und neuronal generierter Videodatensätze sowie das Co-Training mit synthetischen und realen Daten. "Our way of co-training with synthetically generated and real-world data sets us from other large-scale VLA efforts."
7. Schlussfolgerungen:

GR00T N1 wird als ein offenes Foundation Model für generalistische humanoide Roboter vorgestellt, das ein Dual-System-Design aufweist, heterogene Trainingsdaten nutzt und mehrere Roboterverkörperungen unterstützt.
Die Evaluierung zeigt starke Generalisierungsfähigkeiten und hohe Dateneffizienz beim Erlernen verschiedener Manipulationsfähigkeiten.
Die Autoren hoffen, dass das offene GR00T-N1-2B Modell, zusammen mit den Trainingsdaten und Simulationsumgebungen, den Fortschritt der Community bei der Entwicklung und dem Einsatz generalistischer humanoider Roboter beschleunigen wird.
8. Beiträge und Danksagungen:

Auflistung der Kernbeitragenden zu verschiedenen Aspekten des Projekts (Modelltraining, reale Roboter und Teleoperation, Simulation, Videogenerierung, Dateninfrastruktur, Compute-Infrastruktur).
Dank an Personen für technische Diskussionen und Feedback.
9. Anhänge:

Details zu Hyperparametern für Pre- und Post-Training.
Beschreibung des verwendeten Datensatzformats (basierend auf LeRobot, erweitert für feinere Modalitätsspezifikation und multiple Annotationsunterstützung).
Zusätzliche Trainingsdetails, einschließlich Auxiliary Object Detection Loss, Details zur neuronalen Trajektoriengenerierung und zum IDM-Modelltraining.
Zusätzliche qualitative Ergebnisse und Visualisierungen.
Vollständige Statistiken der Pre-Training-Datensätze.
Zitate von besonderer Bedeutung:

"To this end, we introduce GR00T N1, an open foundation model for humanoid robots." (Abstract)
"We advocate for a full-stack solution that integrates the three key ingredients: hardware, models, and data." (Einleitung)
"The vision-language module (System 2) interprets the environment through vision and language instructions. The subsequent diffusion transformer module (System 1) generates fluid motor actions in real time. Both modules are tightly coupled and jointly trained end-to-end." (Abstract - Beschreibung der Architektur)
"To mitigate the “data island” problem mentioned earlier, we structure the VLA training corpora as a data pyramid..." (GR00T N1 Foundation Model - Datenpyramide)
"With a unified model and single set of weights, GR00T N1 can generate diverse manipulation behaviors using single-arm, bimanual, and humanoid embodiments." (GR00T N1 Foundation Model)
"We hope that our open GR00T-N1-2B model, alongside its training datasets and simulation environments, will accelerate the community’s progress toward building and deploying generally capable humanoid robots in the wild." (Schlussfolgerung)
Dieses Briefing-Dokument bietet einen umfassenden Überblick über die wichtigsten Aspekte des GR00T N1 Foundation Models, wie im Whitepaper beschrieben. Es hebt die innovative Architektur, die umfassende Trainingsdatenstrategie und die vielversprechenden Ergebnisse in Simulation und realer Welt hervor.

