# Cosmos Transfer
Dieses Dokument stellt Cosmos-Transfer1 vor, ein neuartiges diffusionsbasiertes Modell zur bedingten Weltgenerierung, das multimodale Eingaben wie Segmentierung und Tiefe adaptiv nutzt, um realistische Simulationen zu erzeugen. Die Arbeit demonstriert die Effektivität des Modells in verschiedenen Anwendungen, einschließlich des Sim-to-Real-Transfers für Robotik und autonomes Fahren, und präsentiert eine Skalierungsstrategie für die Inferenz in Echtzeit auf NVIDIA-Hardware. Das zugehörige Modell und der Code sind öffentlich zugänglich, um die Forschung in diesem Bereich zu fördern.

### Welche potenziellen Anwendungen in Bereichen wie Robotik und autonomes Fahren werden durch die Fähigkeiten von Cosmos-Transfer1 demonstriert?
Die Fähigkeiten von **Cosmos-Transfer1** demonstrieren mehrere **potenzielle Anwendungen in den Bereichen Robotik und autonomes Fahren**.

Im Bereich der **Robotik** wird Cosmos-Transfer1 für die **Generierung von Sim2Real-Daten** eingesetzt. Die Verfügbarkeit großer, qualitativ hochwertiger Daten ist in der Robotikforschung entscheidend. Cosmos-Transfer1 kann genutzt werden, um **synthetische Daten realistischer und vielfältiger zu gestalten**, wodurch die **Domain-Lücke zwischen Simulation und realer Welt verringert** wird.

*   Cosmos-Transfer1 kann **simulierte Robotervideos verbessern**, indem es Beleuchtung, Farbe, Textur und feine Details der Szene anpasst und gleichzeitig die physikalisch plausiblen Roboterbewegungen beibehält.
*   Es kann die **Komplexität von Szenen erhöhen**, indem es neue Hintergrundobjekte einführt und so die ökologische Validität der Daten verbessert.
*   Durch die Nutzung von Cosmos-Transfer1 können Forscher die **Robustheit und Generalisierung von Modellen verbessern**, die in der Simulation trainiert wurden, was eine effektivere Implementierung in realen Robotikumgebungen ermöglicht.
*   In einer Fallstudie wurde gezeigt, dass Cosmos-Transfer1 mit **spatiotemporalen Kontrollkarten** die **Fidelity des Vordergrundroboters verbessern** kann, indem beispielsweise die Form und das Aussehen erhalten bleiben, während der Hintergrund modifiziert wird. Es wurden verschiedene Gewichtungsschemata für Modalitäten wie Edge, Vis und Segmentation angewendet, um unterschiedliche Ziele zu erreichen, z. B. die Beibehaltung der Roboterform bei Variationen des Aussehens.

Im Bereich des **autonomen Fahrens** wird Cosmos-Transfer1 für die **Datenanreicherung** demonstriert. Reale Fahrdaten weisen oft eine Long-Tail-Verteilung auf, wodurch sicherheitskritische Randfälle unterrepräsentiert sein können.

*   Cosmos-Transfer1 ermöglicht es Entwicklern autonomer Fahrzeuge, den **Nutzen realer Randfälle zu maximieren**, indem zahlreiche **nützliche visuelle Variationen** für das Testen und Training generiert werden.
*   Es kann verwendet werden, um **systematisch herausfordernde und seltene Szenarien zu untersuchen**, was sowohl die Testabdeckung als auch die Trainingseffektivität verbessert.
*   Durch die **Kombination verschiedener Kontrollsignale wie HD-Karten und LiDAR** kann Cosmos-Transfer1 realistischere und genauere Fahrszenen generieren, beispielsweise durch die Korrektur von Fahrspurmarkierungen und die Synthese detaillierter Objekte.
*   Es wurde gezeigt, dass das Modell gut auf **synthetische LiDAR-Daten generalisiert**, obwohl es auf realen LiDAR-Daten trainiert wurde, und gleichzeitig Weltwissen aus dem Internet-Pretraining beibehält, was die **visuelle Vielfalt simulierter Szenen erhöhen** kann.
*   Die quantitative Bewertung zeigte, dass die Integration von HD-Karten und LiDAR zu einer **höheren Genauigkeit der Fahrspuren** führt, was für AV-Entwickler entscheidend ist, während gleichzeitig die Genauigkeit der 3D-Objekterkennung beibehalten wird.

Zusammenfassend lässt sich sagen, dass Cosmos-Transfer1 durch seine Fähigkeit zur **kontrollierbaren und qualitativ hochwertigen Weltgenerierung** mit verschiedenen multimodalen Eingaben das Potenzial hat, die **Erzeugung von Trainingsdaten für Robotik und autonomes Fahren erheblich zu verbessern**, die **Domain-Lücke zu schließen** und die **Vielfalt und Repräsentation kritischer Szenarien zu erhöhen**. Die **adaptive Gewichtung** verschiedener Eingabemodalitäten ermöglicht eine **feingranulare Kontrolle** über den Generierungsprozess und die resultierenden Anwendungen. Die Möglichkeit der **Echtzeit-Inferenz** mit der NVIDIA GB200 NVL72-Architektur unterstreicht die praktische Anwendbarkeit von Cosmos-Transfer1.


1. Einführung
Dieses Dokument fasst die wichtigsten Ideen und Fakten aus dem Forschungsartikel "Cosmos-Transfer1: Bedingte Weltgenerierung mit adaptiver multimodaler Steuerung" von NVIDIA zusammen. Der Artikel präsentiert ein neues Modell zur bedingten Generierung von Weltsimulationen, das auf mehreren räumlichen Steuerungseingaben unterschiedlicher Modalitäten wie Segmentierung, Tiefe und Kanten basiert.

Kernidee: Cosmos-Transfer1 ist ein diffusionsbasiertes Modell, das darauf abzielt, hochgradig kontrollierbare Weltsimulationen zu erzeugen und somit Anwendungsfälle wie Sim2Real in der physikalischen KI (z. B. Robotik, autonomes Fahren) zu verbessern.

Wichtigste Punkte:

Multimodale Steuerung: Das Modell nutzt verschiedene räumliche Eingabemodalitäten (Segmentierung, Tiefe, Kanten), um die Generierung der Weltsimulation zu beeinflussen.
Adaptive und anpassbare Steuerung: Das Steuerungsschema ist so konzipiert, dass verschiedene Bedingungseingaben an unterschiedlichen räumlichen Positionen unterschiedlich gewichtet werden können. Dies ermöglicht eine präzise und flexible Kontrolle über den Generierungsprozess.
Anwendungen: Die Technologie findet Anwendung in verschiedenen Bereichen der physikalischen KI, insbesondere bei der Überbrückung der synthetisch-realen Domänengap (Sim2Real) für Robotik und der Datenanreicherung für autonomes Fahren.
Echtzeit-Inferenz: Der Artikel demonstriert eine Inferenzskalierungsstrategie, die es ermöglicht, Welten in Echtzeit mit einem NVIDIA GB200 NVL72 Rack zu generieren.
Open Source: Die Modelle und der Code werden unter https://github.com/nvidia-cosmos/cosmos-transfer1 öffentlich zugänglich gemacht, um die Forschung in diesem Bereich zu beschleunigen.
Zitat: "We introduce Cosmos-Transfer1, a conditional world generation model that can generate world simula-tions based on multiple spatial control inputs of various modalities such as segmentation, depth, and edge."

2. Vorläufige Informationen
Cosmos-Transfer1 baut auf dem bereits existierenden Weltmodell Cosmos-Predict1 (NVIDIA, 2025) auf. Das grundlegende Element eines Diffusionsmodells ist ein Denoiser, der üblicherweise durch eine Transformer-basierte Architektur (DiT) implementiert wird.

Wichtigste Punkte:

Basismodell: Cosmos-Predict1 verwendet eine DiT-Architektur, die darauf trainiert ist, Rauschen in verrauschten Video-Tokens vorherzusagen.
ControlNet-Erweiterung: Cosmos-Transfer1 erweitert dieses Basismodell durch ein neuartiges ControlNet-Design, das mehrere Kontrollzweige für verschiedene Modalitäten hinzufügt.
Getrennte Trainings und Fusion: Die Kontrollzweige für jede Modalität werden separat trainiert und erst zur Inferenzzeit zusammengeführt.
Adaptivität durch Kontrollkarten: Die multimodale Steuerung ist räumlich und zeitlich adaptiv durch die Anwendung einer spatiotemporalen Kontrollkarte auf die Ausgaben der Kontrollzweige. Diese Karte legt das Gewicht jeder Modalität an jeder räumlichen Position und zu jedem Zeitpunkt fest.
Zitat: "Our model is built on top of Cosmos-Predict1 (NVIDIA, 2025), which consists of a set of diffusion transformer-based (DiT-based) (Peebles and Xie, 2023) world models. We add multimodal control branches to the DiT through a novel ControlNet design (Zhang et al., 2023). We build a control branch per modality. If there are multimodal video inputs, then we will have control branches. We train the control branches separately and fuse them in the inference time."

3. Methode
Cosmos-Transfer1 ist ein diffusionsbasierter, multimodal steuerbarer Weltengenerator, der durch Nachtraining des Cosmos-Predict1-Diffusionsweltmodells entwickelt wurde.

Wichtigste Punkte:

Multimodale Eingaben: Das Modell kann durch verschiedene bedingte Eingaben (c1, c2, ..., cn) gesteuert werden, die unterschiedliche Modalitäten repräsentieren.
Spatiotemporale Kontrollkarte (w): Eine zusätzliche Kontrollmöglichkeit bietet die spatiotemporale Kontrollkarte, die es ermöglicht, den Einfluss jeder Modalität an verschiedenen räumlichen und zeitlichen Stellen fein granular anzupassen.
Elementweise Multiplikation: Die Aktivierungen der Kontrollzweige (hᵢⱼ) werden mit den entsprechenden Gewichten der Kontrollkarte (wᵢ) elementweise multipliziert, um die gewichteten Aktivierungen zu erhalten, die dann dem Hauptgenerierungszweig hinzugefügt werden.
Flexible Ableitung der Kontrollkarte: Die spatiotemporale Kontrollkarte kann manuell entworfen, durch heuristische Regeln abgeleitet oder durch ein separates neuronales Modul vorhergesagt werden.
Normalisierung der Gewichte: An jeder spatiotemporalen Position, an der die Summe der Gewichte der verschiedenen Modalitäten größer als eins ist, werden die Gewichte normalisiert, sodass ihre Summe eins ergibt.
Vorteile des separaten Trainings: Das separate Training der Kontrollzweige ist speichereffizienter, ermöglicht das Training mit unterschiedlichen Datensätzen für verschiedene Modalitäten und bietet mehr Flexibilität beim Hinzufügen oder Entfernen von Modalitäten zur Inferenzzeit.
Zitat: "The proposed Cosmos-Transfer1 is a diffusion-based multimodal controllable world generator. It is constructed by post-training the Cosmos-Predict1 diffusion world model (NVIDIA, 2025). Let c1, c2, ..., c be conditional inputs, representing different modalities. Cosmos-Transfer1 learns to leverage these input videos to generate the world simulation."

4. Modalitäten und Training
Die erste Implementierung von Cosmos-Transfer1 ist das nachgeschulte Cosmos-Predict1-7B-Video2World-Modell, genannt Cosmos-Transfer1-7B. Eine spezielle Version für autonome Fahrzeuge, Cosmos-Transfer1-7B-Sample-AV, wurde ebenfalls entwickelt und basiert auf einer für Dashcam-Videos feinabgestimmten Version von Cosmos-Predict1-7B-Video2World.

Wichtigste Modalitäten für Cosmos-Transfer1-7B:

Vis (Verwischtes Visuelles): Ein durch bilaterialen Blur erzeugtes verwischtes Video, nützlich zur Beibehaltung von Farben und groben Formen bei Änderung der Texturdetails.
Edge (Kante): Canny-Kanten, extrahiert aus den Video-Frames, zur Beibehaltung der Szenenstruktur bei freier Gestaltung des Inhalts.
Depth (Tiefe): Tiefenkarten, berechnet mit DepthAnything2, zur Erhaltung der 3D-Geometrie der Szene.
Segmentation (Segmentierung): Semantische Segmentierungsmasken, generiert mit GroundingDino und SAM2, zur Beibehaltung des ursprünglichen Layouts bei freier Inhaltsgenerierung.
Training von Cosmos-Transfer1-7B:

Jeder Kontrollzweig wurde mit 1024 NVIDIA H100 GPUs für 2 bis 4 Wochen trainiert.
Die Inferenz erzeugt ein 5-sekündiges 1280x704p Video mit 24 fps (ca. 56.000 Tokens).
Modalitäten für Cosmos-Transfer1-7B-Sample-AV (für autonomes Fahren):

HDMap: Hochdetaillierte Straßenkarten mit Informationen zu Fahrspuren, Begrenzungen, Ampeln, etc., ergänzt durch 3D-Bounding Boxes dynamischer Objekte.
LiDAR: Temporär interpolierte und verdichtete LiDAR-Scans, projiziert auf die Kameraframes, zur Erhaltung semantischer Details der Fahrszene.
Datensatz für Cosmos-Transfer1-7B-Sample-AV:

Ein neu kuratierter, hochwertiger Datensatz namens Real Driving Scene HQ (RDS-HQ) mit 360 Stunden an Surrounding-View-Videoclips, korrespondierenden 10 Hz LiDAR-Scans und dichten Bildunterschriften.
4KUpscaler:

Eine spezielle Version, Cosmos-Transfer1-7B-4KUpscaler, wurde für das Hochskalieren von Videos von 720p auf 4k-Auflösung trainiert, wobei realistische Reflexionen und schärfere Texturen hinzugefügt werden. Die Inferenz erfolgt patchbasiert mit überlappenden Regionen, um nahtlose Ausgabevideos zu gewährleisten.
5. Evaluierungen
Eine umfassende Evaluierung von Cosmos-Transfer1 wurde durchgeführt, um die Leistungsfähigkeit und die verschiedenen Aspekte des Modells zu analysieren.

Evaluationsdatensatz:

TransferBench: Ein neuer Evaluationsdatensatz mit 600 Beispielen aus drei Schlüsselszenarien der physikalischen KI: Robotermanipulationen (AgiBot World), Fahren (OpenDV) und egozentrische Alltagsszenen (Ego-Exo-4D).
Metriken:

Adherence to Control Input Signals (Übereinstimmung mit Steuerungseingaben):Blur SSIM (für Vis Alignment)
Edge F1 (für Edge Alignment)
Depth si-RMSE (für Depth Alignment)
Mask mIoU (für Segmentation Alignment)
Diversity (Diversität): Diversity-LPIPS
Overall Generation Quality (Gesamtgenerierungsqualität): Quality Score (DOVER-technical score)
5.1. Unimodal versus Multimodal:

Vergleich von Einzelkontrollmodellen (nur eine Modalität aktiv) mit multimodalen Modellen (alle oder mehrere Modalitäten mit gleichen räumlichen Gewichten).
Ergebnisse zeigen, dass Einzelkontrollmodelle in der Regel die besten Ergebnisse für ihre spezifische Ausrichtungsmetrik erzielen, während das multimodale Modell mit uniformen Gewichten eine insgesamt hohe Qualität und eine gute Tiefenrekonstruktion aufweist.
Modalitäten mit dichteren Strukturinformationen (Blur Visual und Edge) führen tendenziell zu geringerer Diversität, während Modalitäten mit weniger dichten Informationen (Depth und Segmentation) höhere Diversität fördern.
Das adaptive multimodale Kontrollmodell integriert komplementäre Merkmale aus allen Modalitäten und führt zu einem ausgewogeneren und genaueren Ergebnis.
Zitat: "As expected, the vis control model achieves the highest Blur SSIM (0.96), reflecting its strength in preserving coarse structure and color. Similarly, the edge control model attains the best Edge F1 score (0.28), underscoring its ability to capture dense structural details in the control input3."

5.2. Fallstudie zu spatiotemporalen Kontrollkarten:

Demonstration der Stärke der vorgeschlagenen spatiotemporalen Kontrollkarte anhand eines "SalientObject"-Algorithmus, der Vordergrund- und Hintergrundbereichen unterschiedliche Modalitätsgewichte zuweist.
Ein VLM wird verwendet, um vorab extrahierte Segmentierungsmasken als Vorder- oder Hintergrund zu klassifizieren.
Beispiel: Vordergrund (Vis und Edge mit Gewicht 0.5), Hintergrund (Depth und Segmentation mit Gewicht 0.5). Dies ermöglicht die Beibehaltung detaillierter Vordergrundobjekte bei gleichzeitiger Diversifizierung des Hintergrunds.
Quantitative Experimente zeigen eine starke Korrelation zwischen den Modalitätsgewichten in Vorder- und Hintergrundbereichen und den entsprechenden Ausrichtungsmetriken.
Das gezielte Erhöhen des Gewichts einer Modalität im entsprechenden Bereich führt zu einer Verbesserung der zugehörigen Metrik.
Das Vertauschen von Modalitäten mit niedriger und hoher Freiheitsgrad im Vorder- und Hintergrund führt zu erwarteten Veränderungen in Diversität und Ausrichtung.
Zitat: "To demonstrate the strength of the proposed spatiotemporal control map, we design a SalientObject algorithm where we give different modalities different weights based on whether the location is from foreground or background."

5.3. Fallstudie zur Robotik Sim2Real Datengenerierung:

Evaluierung von Cosmos-Transfer1 für die Generierung von Robotikdaten zur Überbrückung der Sim2Real-Domänengap.
Verwendung eines kleinen simulierten Datensatzes mit 20 Robotermanipulationsszenarien in einer einfachen Küchenszene (NVIDIA Omniverse und Isaac Lab).
Für jede Szene wurden sechs verschiedene Textprompts erstellt, die Beleuchtung, Szenendetails und Betriebsumgebungen beschreiben.
Zusätzlich zu RGB-Videos wurden Segmentierungs- und Tiefenkarten ausgegeben.
Zwei Konfigurationen für die spatiotemporale Kontrollkarte wurden getestet:
Setting 1: Edge und Vis für den Vordergrund (Roboter), Segmentation für den Hintergrund.
Setting 2: Nur Edge für den Vordergrund, Segmentation für den Hintergrund.
Die Ergebnisse zeigen, dass Einzelmodalmodelle in ihren spezifischen Metriken höhere Werte erzielen, die multimodalen Einstellungen jedoch eine bessere Gesamtqualität, Diversität und Erhaltung des Vordergrundroboters aufweisen.
Cosmos-Transfer1 verbessert die Photorealismus und Diversität der simulierten Robotikvideos erheblich.
Zitat: "To evaluate Cosmos-Transfer1 for robotics data generation, we curated a small simulated dataset of 20 robot manipulation scenarios in a basic kitchen scene using NVIDIA Omniverse and Isaac Lab."

5.4. Fallstudie zur Datenanreicherung für autonomes Fahren:

Nutzung von Cosmos-Transfer1 zur Anreicherung und Diversifizierung von realen Fahrdaten, insbesondere für sicherheitskritische Grenzfälle.
Konditionierung des Modells auf Interventionen in Szenarioformaten zur Generierung visueller Variationen für das Testen und Trainieren autonomer Fahrzeuge.
Vergleich der Generierungsergebnisse bei Verwendung einzelner (Depth oder Segmentation) und kombinierter (uniform gewichteter) Kontrollsignale.
Die Kombination von Depth und Segmentation kann fehlende Details wiederherstellen und realistischere Szenen erzeugen.
Vergleich von Einzelkontrollsignalen (HDMap oder LiDAR) mit deren kombinierter adaptiver multimodaler Steuerung für Cosmos-Transfer1-7B-Sample-AV (Gewichte: w_map = 0.3, w_lidar = 0.7).
Die Fusion von LiDAR und HDMap verbessert die Gesamtrealismus der generierten Fahrspurlayouts und synthetisiert detailliertere Objekte.
Quantitative Auswertung mit 3D-Bbox mAP, Lane mIoU und photometrischem Reprojektionsfehler zeigt, dass Cosmos-Transfer1-7B-Sample-AV [LiDAR] die besten 3D-Objekterkennungs-Scores erzielt, während die Kombination von HDMap und LiDAR die höchste Fahrspurgenauigkeit bei vergleichbarer 3D-Objekterkennung und geringem Reprojektionsfehler bietet.
Das Modell generalisiert gut auf synthetische LiDAR-Daten, die mit NVIDIA Omniverse Blueprint für AV-Simulationen erzeugt wurden, und kann Szenen und Effekte generieren, die im realen Trainingsdatensatz nicht vorhanden sind.
Zitat: "By leveraging Cosmos-Transfer1, autonomous vehicle developers can maximize the utility of real-world edge cases, ultimately leading to enriched and diversified driving data."

6. Echtzeit-Inferenz
Der Artikel beschreibt eine Implementierung von Cosmos-Transfer1-7B, die durch die Nutzung des neuen NVIDIA GB200 NVL72 Systems Echtzeit-Inferenzleistung erreicht.

Wichtigste Punkte:

GB200 NVL72 System: Enthält 36 Grace CPUs und 72 Blackwell GPUs mit Any-to-Any NVLink-Netzwerk, ideal für Modellparallelität.
Parallelisierungsstrategie: Reine Datenparallelität in nicht-Attention-Layern und Head-Parallelität in Attention-Layern.
Sharding der Token-Sequenz: Die gesamte 56K-Token-Sequenz für ein 5-sekündiges 720p-Video wird auf die GPUs verteilt.
All-to-All-Kommunikation in Attention: Jede GPU operiert auf der gesamten Token-Sequenz aus einem einzelnen Attention Head.
Verteilung der Workload für positive und negative Konditionierung: Auf zwei GPU-Gruppen aufgeteilt.
Ergebnisse: Eine nahezu 40-fache Beschleunigung der Diffusionslaufzeit beim Übergang von 1 zu 64 GPUs.
Echtzeit-Durchsatz: Die Generierung eines 5-sekündigen Videos dauert mit 64 GPUs nur 4,2 Sekunden.
Zitat: "We report generation times when using different numbers of GPUs in Tab. 5. As shown, our parallelism strategy exhibits an approximately 40X speedup when going from 1 to 64 GPUs, when we only consider diffusion runtime, which is over 99% of the workload, and the only workload we parallelize across GPUs."

7. Verwandte Arbeiten
Der Artikel diskutiert verwandte Forschungsarbeiten in den Bereichen visueller Domänentransfer, räumliche Steuerung für Diffusionsmodelle und die Verbesserung der Simulation mit generativen Modellen.

Wichtigste Punkte:

Visueller Domänentransfer: Zahlreiche Studien zur Konvertierung abstrakter Darstellungen in photorealistische Bilder und Videos, insbesondere im Bereich der semantischen Bildsynthese.
Räumliche Steuerung für Diffusionsmodelle: Methoden zur Verbesserung der räumlichen Kontrollierbarkeit von Diffusionsmodellen, einschließlich trainingfreier Ansätze und Techniken, die zusätzliches Training erfordern (z. B. ControlNet und seine Erweiterungen).
Verbesserung der Simulation mit generativen Modellen: Einsatz generativer Modelle (insbesondere GANs und Diffusionsmodelle) zur Steigerung des Realismus, der Diversität und der Nützlichkeit von Simulationen für die physikalische KI.
8. Fazit
Cosmos-Transfer1 ist ein leistungsstarkes diffusionsbasiertes Modell für die bedingte und multimodale steuerbare Weltgenerierung. Durch die adaptive Gewichtung verschiedener Bedingungseingaben ermöglicht es eine hochgradig kontrollierbare Generierung und verbessert die Qualität der Simulationen. Die Evaluierungen zeigen die Wirksamkeit von Cosmos-Transfer1 bei der Überbrückung der Sim2Real-Domänengap in der Robotik und der Datenanreicherung für autonomes Fahren. Die Möglichkeit der Echtzeit-Inferenz auf NVIDIA GB200 NVL72 Systemen unterstreicht die praktische Anwendbarkeit der Technologie. Die Veröffentlichung des Codes und der Modelle als Open Source soll die Weiterentwicklung der Forschung im Bereich der physikalischen KI fördern.

Zitat: "We introduced Cosmos-Transfer1, a diffusion-based conditional world model for multimodal controllable world generation. By introducing multimodal control branches to Cosmos-Predict1 (NVIDIA, 2025) with an adaptive weighting scheme, Cosmos-Transfer1 enables highly controllable world generation and improves generation quality."



### Häufig gestellte Fragen zu Cosmos-Transfer1
1. Was ist Cosmos-Transfer1 und was sind seine Hauptfunktionen?
Cosmos-Transfer1 ist ein bedingtes Weltgenerierungsmodell, das darauf ausgelegt ist, realistische Welt-Simulationen basierend auf verschiedenen räumlichen Steuerungseingaben wie Segmentierung, Tiefe und Kanten zu erzeugen. Sein Design ermöglicht eine adaptive und anpassbare räumliche Konditionierung, bei der verschiedene Eingabemodalitäten an unterschiedlichen räumlichen Positionen unterschiedlich gewichtet werden können. Dies ermöglicht eine hochgradig kontrollierbare Weltgenerierung und ist nützlich für verschiedene Anwendungsfälle der Welt-zu-Welt-Übertragung, einschließlich Sim2Real-Szenarien in der Robotik und der Anreicherung von Daten für autonomes Fahren. Das Modell basiert auf einem Diffusionsansatz und ist darauf ausgelegt, qualitativ hochwertige und vielfältige Videos von Welt-Simulationen zu generieren.

2. Wie funktioniert die adaptive multimodale Steuerung in Cosmos-Transfer1?
Die adaptive multimodale Steuerung in Cosmos-Transfer1 wird durch die Verwendung mehrerer Kontrollzweige erreicht, einer für jede Eingabemodalität (z. B. Segmentierung, Tiefe, Kanten). Diese Zweige werden separat trainiert und zur Inferenzzeit fusioniert. Die Adaptivität wird durch eine spatiotemporale Kontrollkarte eingeführt, die jedem Pixel zu jedem Zeitpunkt ein Gewicht für jede Modalität zuweist. Diese Gewichte bestimmen, wie stark jede Modalität die Generierung des Videos an dieser spezifischen räumlichen und zeitlichen Position beeinflusst. Benutzer können diese Kontrollkarten manuell entwerfen, sie durch heuristische Regeln ableiten oder ein separates neuronales Modul trainieren, um sie vorherzusagen. Diese flexible Gewichtung ermöglicht eine präzise Steuerung der generierten Inhalte, beispielsweise die Bevorzugung der Tiefeninformationen zur Erhaltung der Szenengeometrie oder die Hervorhebung von Kanten zur Bewahrung feiner Details.

3. Welche verschiedenen Eingabemodalitäten werden von Cosmos-Transfer1 unterstützt und wofür sind sie nützlich?
Cosmos-Transfer1 unterstützt verschiedene Eingabemodalitäten, darunter:

Visuell (Blur): Eine verwischte Version des Eingabevideos. Nützlich, um die ursprünglichen Farben und groben Formen beizubehalten, während Texturdetails geändert oder hinzugefügt werden. Ideal für die Übertragung von CG-Renderings zu realistischen Videos mit verbesserten Kantendetails und Klarheit.
Kante: Canny-Kanten, die aus dem Eingabevideo extrahiert werden. Nützlich, um die Gesamtstruktur der Szene beizubehalten und dem Modell mehr Freiheit bei der Ausgestaltung der restlichen Details zu geben.
Tiefe: Tiefenkarten, die aus dem Eingabevideo berechnet werden. Besonders nützlich, wenn die 3D-Geometrie der Eingabewelt erhalten bleiben soll.
Segmentierung: Semantische Segmentierungsmasken, die Objekte im Video kennzeichnen. Vorteilhaft, wenn das ursprüngliche Segmentierungslayout der Szene beibehalten werden soll, während neue Videoinhalte mit hoher Flexibilität generiert werden. Für Anwendungen im autonomen Fahren unterstützt Cosmos-Transfer1 auch HD-Karten (zur Beibehaltung des Straßenlayouts und zur Simulation verschiedener Ego-Fahrzeugtrajektorien) und LiDAR-Daten (zur Bewahrung semantischer Details und zur Ermöglichung von Änderungen der Umgebungsbedingungen und der Beleuchtung).
4. Wie wird Cosmos-Transfer1 trainiert und welche Vorteile bietet der separate Trainingsansatz für die Kontrollzweige?
Cosmos-Transfer1 wird durch ein Nachtraining auf dem bestehenden Weltmodell Cosmos-Predict1 aufgebaut. Die einzelnen Kontrollzweige für jede Modalität werden separat trainiert und erst zur Inferenzzeit zusammengeführt. Dieser Ansatz bietet mehrere Vorteile:

Speichereffizienz: Es wird weniger Speicher benötigt, da jeweils nur ein Kontrollzweig während des Trainings in den Speicher passen muss.
Flexibilität bei den Trainingsdaten: Verschiedene Modalitäten können mit unterschiedlichen Datensätzen trainiert werden, da für bestimmte Modalitäten möglicherweise keine gepaarten Daten verfügbar sind.
Erweiterbarkeit: Neue Modalitäten können nach Abschluss des Trainings einfach zur Inferenzzeit hinzugefügt oder entfernt werden. Während des Trainings der Kontrollzweige werden die Gewichte des Basismodells eingefroren, und nur die zusätzlichen Kontrollzweige werden optimiert.
5. Wie wurde die Leistung von Cosmos-Transfer1 evaluiert und welche Ergebnisse wurden erzielt?
Die Leistung von Cosmos-Transfer1 wurde anhand von drei Hauptaspekten evaluiert: 1) Übereinstimmung mit den Steuerungseingabesignalen, 2) Generierungsvielfalt und 3) Allgemeine Generierungsqualität. Für die quantitative Bewertung wurde der TransferBench-Datensatz mit 600 Beispielen aus den Bereichen Robotik, autonomes Fahren und Ego-zentrische Alltagsszenen verwendet. Es wurden verschiedene Metriken eingesetzt, darunter Blur SSIM, Edge F1, Depth si-RMSE und Mask mIoU zur Messung der Übereinstimmung, Diversity-LPIPS zur Messung der Vielfalt und der DOVER-technische Score zur Bewertung der visuellen Qualität. Die Ergebnisse zeigten, dass Cosmos-Transfer1 in der Lage ist, die Szenenstruktur aus den Eingabebedingungen effektiv zu erhalten und gleichzeitig eine feingranulare Kontrolle zu ermöglichen. Multimodale Steuerung mit adaptiven Gewichtungen führte oft zu einer besseren Gesamtqualität und einem ausgewogeneren Ergebnis im Vergleich zur Verwendung einzelner Modalitäten. Fallstudien in der Robotik (Sim2Real) und im autonomen Fahren demonstrierten die praktische Anwendbarkeit und die Fähigkeit des Modells, nützliche und vielfältige Daten zu generieren.

6. Wie kann Cosmos-Transfer1 zur Verbesserung von Sim2Real-Anwendungen in der Robotik eingesetzt werden?
In der Robotik ist die Diskrepanz zwischen simulierten und realen Daten eine große Herausforderung. Cosmos-Transfer1 kann eingesetzt werden, um diese Domänenlücke zu verkleinern, indem es synthetische Bilder und Videos aus Simulatoren (wie NVIDIA Omniverse und Isaac Lab) in realistischere Darstellungen umwandelt. Durch die Verwendung von Eingabemodalitäten wie Segmentierung und Tiefe aus der Simulation und die Anwendung spatiotemporaler Kontrollkarten kann Cosmos-Transfer1 die Form und Dynamik von Robotern in simulierten Szenen beibehalten, während gleichzeitig die Hintergrundumgebung und andere visuelle Details realistischer und vielfältiger gestaltet werden. Dies ermöglicht das Training von Robotermodellen auf einer größeren und realistischeren Datenbasis, was die Übertragung des Gelernten auf reale Roboter verbessern kann.

7. Wie kann Cosmos-Transfer1 die Datenerzeugung und -anreicherung für autonomes Fahren unterstützen?
Im Bereich des autonomen Fahrens können mit Cosmos-Transfer1 realistische und vielfältige Fahrdaten generiert werden, insbesondere um sicherheitskritische oder seltene Szenarien abzudecken, die in realen Datensätzen unterrepräsentiert sein können. Durch die Konditionierung des Modells mit Eingabemodalitäten wie HD-Karten und LiDAR-Daten können spezifische Szenariendesigns und Interventionen (z. B. Straßensperrungen, widrige Wetterbedingungen, verschiedene Tageszeiten) simuliert und visuell dargestellt werden. Dies ermöglicht eine systematische Erforschung herausfordernder Situationen für das Testen und Trainieren autonomer Fahrzeuge. Die Fähigkeit des Modells, auch auf synthetischen LiDAR-Daten zu generalisieren und Weltwissen aus dem Vortraining einzubringen, erweitert die Möglichkeiten zur Generierung vielfältiger und realistischer Simulationsdaten.

8. Welche Fortschritte wurden beim Erreichen der Echtzeit-Inferenz mit Cosmos-Transfer1 erzielt?
Durch die Nutzung des NVIDIA GB200 NVL72 Systems und die Implementierung einer speziell entwickelten Parallelisierungsstrategie konnte für Cosmos-Transfer1-7B eine Echtzeit-Inferenzleistung demonstriert werden. Die Strategie kombiniert Datenparallelität in nicht-Aufmerksamkeitsschichten mit Head-Parallelität in Aufmerksamkeitsschichten. Das Modell wird über mehrere Blackwell-GPUs verteilt, wobei jede GPU eine vollständige Kopie des Modells speichert. Die Verarbeitung der Token-Sequenz wird auf die GPUs aufgeteilt, und die Kommunikation beschränkt sich hauptsächlich auf die Aufmerksamkeitsberechnung. Mit 64 B200-GPUs konnte eine Beschleunigung von etwa dem 40-Fachen im Vergleich zur Verwendung einer einzelnen GPU erreicht werden, was zu einer End-to-End-Generierungszeit von nur 4,2 Sekunden für ein 5-sekündiges 720p-Video führte und somit Echtzeit-Fähigkeiten demonstriert.
