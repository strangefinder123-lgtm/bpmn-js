
[DEDE.txt](https://github.com/user-attachments/files/23801881/DEDE.txt)
<?xml version="1.0" encoding="UTF-8"?>
<bpmn:definitions xmlns:bpmn="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" id="Definitions_Ist_Konsolidiert" targetNamespace="http://bpmn.io/schema/bpmn">
  <bpmn:collaboration id="Collaboration_Bestellprozess_Ist_Konsolidiert">
    <bpmn:participant id="Participant_Bestellung" name="BESTELLPROZESS (Filiale/KPI) - (Details Abbildung 10)" processRef="Process_Bestellung" />
    <bpmn:participant id="Participant_Kontrolle" name="BESTELLKONTROLLE (VKI/Geschäftsführung) - Sporadisch" processRef="Process_Kontrolle" />
    
    <bpmn:messageFlow id="MessageFlow_BestellungAnVKI" name="Finale Bestellung übermittelt" sourceRef="Task_FinaleUebermittlung" targetRef="StartEvent_Empfangen" />
  </bpmn:collaboration>

  <bpmn:process id="Process_Bestellung" isExecutable="false">
    <bpmn:laneSet id="LaneSet_Bestellung">
      <bpmn:lane id="Lane_ERP" name="ERP-System (Grundbestellung)">
        <bpmn:flowNodeRef>StartEvent_1</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_AutoBerechnung</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_StandardUebermitteln</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_Filiale" name="Verkauf / Filialleitung">
        <bpmn:flowNodeRef>Task_Abrufen</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_PruefenAnpassen</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Analyse</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_FinaleUebermittlung</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Event_EndeBestellung</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_KPI" name="KPI-/BI-System">
        <bpmn:flowNodeRef>Task_KennzahlenBereitstellen</bpmn:flowNodeRef>
      </bpmn:lane>
    </bpmn:laneSet>
    
    <bpmn:startEvent id="StartEvent_1" name="Start: 2 Tage vor Lieferdatum">
      <bpmn:outgoing>Flow_1</bpmn:outgoing>
      <bpmn:timerEventDefinition id="TimerEventDefinition_1" />
    </bpmn:startEvent>
    <bpmn:task id="Task_AutoBerechnung" name="Automatische Grundbestellung berechnen (Daten: Verkäufe + Retoure Vorwoche, ABC-Liste, Bestelldaten/Mindestbestellmenge, ext. Faktoren)">
      <bpmn:incoming>Flow_1</bpmn:incoming>
      <bpmn:outgoing>Flow_2</bpmn:outgoing>
    </bpmn:task>
    <bpmn:task id="Task_StandardUebermitteln" name="Elektronische Übermittlung Standardbestellung an Filiale">
      <bpmn:incoming>Flow_2</bpmn:incoming>
      <bpmn:outgoing>Flow_3</bpmn:outgoing>
    </bpmn:task>
    
    <bpmn:task id="Task_Abrufen" name="Standardbestellung im System abrufen / Daten importieren (Ggf. Nachfrage fehlender Daten)">
      <bpmn:incoming>Flow_3</bpmn:incoming>
      <bpmn:incoming>Flow_RueckfrageFiliale</bpmn:incoming>
      <bpmn:outgoing>Flow_4</bpmn:outgoing>
    </bpmn:task>
    <bpmn:task id="Task_KennzahlenBereitstellen" name="Kennzahlen bereitstellen (Umsatz, Retouren, Top-Produkte)">
      <bpmn:incoming>Flow_4</bpmn:incoming>
      <bpmn:outgoing>Flow_5</bpmn:outgoing>
    </bpmn:task>
    <bpmn:task id="Task_Analyse" name="KPI-Dashboard aufrufen &amp; analysieren">
      <bpmn:incoming>Flow_5</bpmn:incoming>
      <bpmn:outgoing>Flow_6</bpmn:outgoing>
    </bpmn:task>
    <bpmn:task id="Task_PruefenAnpassen" name="Bestellung prüfen, ggf. ergänzen und anpassen (Telefonische Übermittlung Kundenbestellung)">
      <bpmn:incoming>Flow_6</bpmn:incoming>
      <bpmn:outgoing>Flow_7</bpmn:outgoing>
    </bpmn:task>
    <bpmn:sendTask id="Task_FinaleUebermittlung" name="Finale Bestellung an VKI senden (Abschluss Bestellung)">
      <bpmn:incoming>Flow_7</bpmn:incoming>
      <bpmn:outgoing>Flow_8</bpmn:outgoing>
    </bpmn:sendTask>
    <bpmn:endEvent id="Event_EndeBestellung" name="Prozessende (Bestellung an VKI gesendet)">
      <bpmn:incoming>Flow_8</bpmn:incoming>
    </bpmn:endEvent>
    
    <bpmn:sequenceFlow id="Flow_1" sourceRef="StartEvent_1" targetRef="Task_AutoBerechnung" />
    <bpmn:sequenceFlow id="Flow_2" sourceRef="Task_AutoBerechnung" targetRef="Task_StandardUebermitteln" />
    <bpmn:sequenceFlow id="Flow_3" sourceRef="Task_StandardUebermitteln" targetRef="Task_Abrufen" />
    <bpmn:sequenceFlow id="Flow_4" sourceRef="Task_Abrufen" targetRef="Task_KennzahlenBereitstellen" />
    <bpmn:sequenceFlow id="Flow_5" sourceRef="Task_KennzahlenBereitstellen" targetRef="Task_Analyse" />
    <bpmn:sequenceFlow id="Flow_6" sourceRef="Task_Analyse" targetRef="Task_PruefenAnpassen" />
    <bpmn:sequenceFlow id="Flow_7" sourceRef="Task_PruefenAnpassen" targetRef="Task_FinaleUebermittlung" />
    <bpmn:sequenceFlow id="Flow_8" sourceRef="Task_FinaleUebermittlung" targetRef="Event_EndeBestellung" />
  </bpmn:process>

  <bpmn:process id="Process_Kontrolle" isExecutable="false">
    <bpmn:laneSet id="LaneSet_Kontrolle">
      <bpmn:lane id="Lane_VKI" name="Verkaufsinnendienst (Verwaltung)">
        <bpmn:flowNodeRef>StartEvent_Empfangen</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_GesamtbestellungAggregieren</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_BestellungenMonitoren</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Gateway_Unstimmigkeiten</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_RueckfrageKlaerung</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>Task_Freigabe</bpmn:flowNodeRef>
        <bpmn:flowNodeRef>EndEvent_KontrolleEnde</bpmn:flowNodeRef>
      </bpmn:lane>
      <bpmn:lane id="Lane_Geschaeftsfuehrung" name="Geschäftsführung">
        <bpmn:flowNodeRef>Task_PruefungKonsistenz</bpmn:flowNodeRef>
      </bpmn:lane>
    </bpmn:laneSet>
    
    <bpmn:startEvent id="StartEvent_Empfangen" name="Bestellung empfangen (Sporadisch)">
      <bpmn:outgoing>Flow_K1</bpmn:outgoing>
      <bpmn:messageEventDefinition id="MessageEventDefinition_1" />
    </bpmn:startEvent>
    
    <bpmn:task id="Task_GesamtbestellungAggregieren" name="Gesamtbestellung aggregieren (inkl. Manuelle Eingabe Kundenbestellung &amp; Überprüfung auf Vollständigkeit)">
      <bpmn:incoming>Flow_K1</bpmn:incoming>
      <bpmn:outgoing>Flow_K2</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_PruefungKonsistenz" name="Vorlage Bestelldaten aller Filialen zur Prüfung (Prüfung auf Konsistenz)">
      <bpmn:incoming>Flow_K2</bpmn:incoming>
      <bpmn:outgoing>Flow_K3</bpmn:outgoing>
    </bpmn:task>

    <bpmn:task id="Task_BestellungenMonitoren" name="Bestellungen monitoren / Plausibilitätscheck (Ggf. Klärung Unstimmigkeiten durch GF)">
      <bpmn:incoming>Flow_K3</bpmn:incoming>
      <bpmn:outgoing>Flow_K4</bpmn:outgoing>
    </bpmn:task>

    <bpmn:exclusiveGateway id="Gateway_Unstimmigkeiten" name="Unstimmigkeiten?">
      <bpmn:incoming>Flow_K4</bpmn:incoming>
      <bpmn:outgoing>Flow_K5_Nein</bpmn:outgoing>
      <bpmn:outgoing>Flow_K5_Ja</bpmn:outgoing>
    </bpmn:exclusiveGateway>

    <bpmn:sendTask id="Task_RueckfrageKlaerung" name="Rückfrage &amp; Klärung mit Filiale (Ggf. Klärung Unstimmigkeiten)">
      <bpmn:incoming>Flow_K5_Ja</bpmn:incoming>
      <bpmn:outgoing>Flow_K6</bpmn:outgoing>
    </bpmn:sendTask>
    
    <bpmn:task id="Task_Freigabe" name="Bestellung für Produktion / Logistik freigeben (Abschluss Bestellung)">
      <bpmn:incoming>Flow_K5_Nein</bpmn:incoming>
      <bpmn:incoming>Flow_K6</bpmn:incoming>
      <bpmn:outgoing>Flow_K7</bpmn:outgoing>
    </bpmn:task>

    <bpmn:endEvent id="EndEvent_KontrolleEnde" name="Kontrollprozess abgeschlossen">
      <bpmn:incoming>Flow_K7</bpmn:incoming>
    </bpmn:endEvent>
    
    <bpmn:sequenceFlow id="Flow_K1" sourceRef="StartEvent_Empfangen" targetRef="Task_GesamtbestellungAggregieren" />
    <bpmn:sequenceFlow id="Flow_K2" sourceRef="Task_GesamtbestellungAggregieren" targetRef="Task_PruefungKonsistenz" />
    <bpmn:sequenceFlow id="Flow_K3" sourceRef="Task_PruefungKonsistenz" targetRef="Task_BestellungenMonitoren" />
    <bpmn:sequenceFlow id="Flow_K4" sourceRef="Task_BestellungenMonitoren" targetRef="Gateway_Unstimmigkeiten" />
    <bpmn:sequenceFlow id="Flow_K5_Nein" name="Nein" sourceRef="Gateway_Unstimmigkeiten" targetRef="Task_Freigabe" />
    <bpmn:sequenceFlow id="Flow_K5_Ja" name="Ja" sourceRef="Gateway_Unstimmigkeiten" targetRef="Task_RueckfrageKlaerung" />
    <bpmn:sequenceFlow id="Flow_K6" sourceRef="Task_RueckfrageKlaerung" targetRef="Task_Freigabe" />
    <bpmn:sequenceFlow id="Flow_K7" sourceRef="Task_Freigabe" targetRef="EndEvent_KontrolleEnde" />
  </bpmn:process>
  
  <bpmndi:BPMNDiagram id="BPMNDiagram_1">
    <bpmndi:BPMNPlane id="BPMNPlane_1" bpmnElement="Collaboration_Bestellprozess_Ist_Konsolidiert">
      <bpmndi:BPMNShape id="Participant_Bestellung_di" bpmnElement="Participant_Bestellung" isHorizontal="true">
        <dc:Bounds x="140" y="80" width="1400" height="350" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_ERP_di" bpmnElement="Lane_ERP" isHorizontal="true">
        <dc:Bounds x="170" y="80" width="1370" height="100" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_Filiale_di" bpmnElement="Lane_Filiale" isHorizontal="true">
        <dc:Bounds x="170" y="180" width="1370" height="150" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_KPI_di" bpmnElement="Lane_KPI" isHorizontal="true">
        <dc:Bounds x="170" y="330" width="1370" height="100" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="StartEvent_1_di" bpmnElement="StartEvent_1">
        <dc:Bounds x="210" y="112" width="36" height="36" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="196" y="155" width="65" height="27" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_AutoBerechnung_di" bpmnElement="Task_AutoBerechnung">
        <dc:Bounds x="300" y="100" width="100" height="60" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_StandardUebermitteln_di" bpmnElement="Task_StandardUebermitteln">
        <dc:Bounds x="460" y="110" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Abrufen_di" bpmnElement="Task_Abrufen">
        <dc:Bounds x="460" y="210" width="100" height="60" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_KennzahlenBereitstellen_di" bpmnElement="Task_KennzahlenBereitstellen">
        <dc:Bounds x="620" y="350" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Analyse_di" bpmnElement="Task_Analyse">
        <dc:Bounds x="780" y="220" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_PruefenAnpassen_di" bpmnElement="Task_PruefenAnpassen">
        <dc:Bounds x="940" y="210" width="100" height="60" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_FinaleUebermittlung_di" bpmnElement="Task_FinaleUebermittlung">
        <dc:Bounds x="1100" y="220" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Event_EndeBestellung_di" bpmnElement="Event_EndeBestellung">
        <dc:Bounds x="1262" y="222" width="36" height="36" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="1245" y="265" width="70" height="27" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNShape>
      
      <bpmndi:BPMNShape id="Participant_Kontrolle_di" bpmnElement="Participant_Kontrolle" isHorizontal="true">
        <dc:Bounds x="140" y="470" width="1400" height="250" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_VKI_di" bpmnElement="Lane_VKI" isHorizontal="true">
        <dc:Bounds x="170" y="470" width="1370" height="150" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Lane_Geschaeftsfuehrung_di" bpmnElement="Lane_Geschaeftsfuehrung" isHorizontal="true">
        <dc:Bounds x="170" y="620" width="1370" height="100" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="StartEvent_Empfangen_di" bpmnElement="StartEvent_Empfangen">
        <dc:Bounds x="210" y="552" width="36" height="36" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="187" y="595" width="83" height="27" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_GesamtbestellungAggregieren_di" bpmnElement="Task_GesamtbestellungAggregieren">
        <dc:Bounds x="300" y="540" width="100" height="60" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_PruefungKonsistenz_di" bpmnElement="Task_PruefungKonsistenz">
        <dc:Bounds x="460" y="650" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_BestellungenMonitoren_di" bpmnElement="Task_BestellungenMonitoren">
        <dc:Bounds x="620" y="550" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Gateway_Unstimmigkeiten_di" bpmnElement="Gateway_Unstimmigkeiten" isMarkerVisible="true">
        <dc:Bounds x="785" y="545" width="50" height="50" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="767" y="523" width="87" height="14" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_RueckfrageKlaerung_di" bpmnElement="Task_RueckfrageKlaerung">
        <dc:Bounds x="760" y="620" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="Task_Freigabe_di" bpmnElement="Task_Freigabe">
        <dc:Bounds x="940" y="550" width="100" height="40" />
      </bpmndi:BPMNShape>
      <bpmndi:BPMNShape id="EndEvent_KontrolleEnde_di" bpmnElement="EndEvent_KontrolleEnde">
        <dc:Bounds x="1102" y="552" width="36" height="36" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="1086" y="595" width="70" height="27" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNShape>

      <bpmndi:BPMNEdge id="Flow_1_di" bpmnElement="Flow_1">
        <di:waypoint x="246" y="130" />
        <di:waypoint x="300" y="130" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_2_di" bpmnElement="Flow_2">
        <di:waypoint x="400" y="130" />
        <di:waypoint x="460" y="130" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_3_di" bpmnElement="Flow_3">
        <di:waypoint x="510" y="150" />
        <di:waypoint x="510" y="210" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_4_di" bpmnElement="Flow_4">
        <di:waypoint x="510" y="270" />
        <di:waypoint x="510" y="370" />
        <di:waypoint x="620" y="370" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_5_di" bpmnElement="Flow_5">
        <di:waypoint x="720" y="370" />
        <di:waypoint x="830" y="370" />
        <di:waypoint x="830" y="260" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_6_di" bpmnElement="Flow_6">
        <di:waypoint x="880" y="240" />
        <di:waypoint x="940" y="240" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_7_di" bpmnElement="Flow_7">
        <di:waypoint x="1040" y="240" />
        <di:waypoint x="1100" y="240" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_8_di" bpmnElement="Flow_8">
        <di:waypoint x="1200" y="240" />
        <di:waypoint x="1262" y="240" />
      </bpmndi:BPMNEdge>
      
      <bpmndi:BPMNEdge id="Flow_K1_di" bpmnElement="Flow_K1">
        <di:waypoint x="246" y="570" />
        <di:waypoint x="300" y="570" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K2_di" bpmnElement="Flow_K2">
        <di:waypoint x="400" y="570" />
        <di:waypoint x="460" y="570" />
        <di:waypoint x="460" y="670" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K3_di" bpmnElement="Flow_K3">
        <di:waypoint x="560" y="670" />
        <di:waypoint x="670" y="670" />
        <di:waypoint x="670" y="590" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K4_di" bpmnElement="Flow_K4">
        <di:waypoint x="720" y="570" />
        <di:waypoint x="785" y="570" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K5_Nein_di" bpmnElement="Flow_K5_Nein">
        <di:waypoint x="835" y="570" />
        <di:waypoint x="940" y="570" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="876" y="552" width="23" height="14" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K5_Ja_di" bpmnElement="Flow_K5_Ja">
        <di:waypoint x="810" y="595" />
        <di:waypoint x="810" y="620" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="820" y="605" width="12" height="14" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K6_di" bpmnElement="Flow_K6">
        <di:waypoint x="860" y="640" />
        <di:waypoint x="990" y="640" />
        <di:waypoint x="990" y="590" />
      </bpmndi:BPMNEdge>
      <bpmndi:BPMNEdge id="Flow_K7_di" bpmnElement="Flow_K7">
        <di:waypoint x="1040" y="570" />
        <di:waypoint x="1102" y="570" />
      </bpmndi:BPMNEdge>
      
      <bpmndi:BPMNEdge id="MessageFlow_BestellungAnVKI_di" bpmnElement="MessageFlow_BestellungAnVKI">
        <di:waypoint x="1150" y="260" />
        <di:waypoint x="1150" y="470" />
        <di:waypoint x="228" y="470" />
        <di:waypoint x="228" y="552" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="750" y="452" width="107" height="14" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNEdge>

      <bpmn:messageFlow id="MessageFlow_VKI_Rueckfrage" name="Rückfrage/Klärung" sourceRef="Task_RueckfrageKlaerung" targetRef="Task_Abrufen" />
      <bpmndi:BPMNEdge id="MessageFlow_VKI_Rueckfrage_di" bpmnElement="MessageFlow_VKI_Rueckfrage">
        <di:waypoint x="810" y="620" />
        <di:waypoint x="810" y="280" />
        <di:waypoint x="560" y="280" />
        <bpmndi:BPMNLabel>
          <dc:Bounds x="670" y="440" width="80" height="14" />
        </bpmndi:BPMNLabel>
      </bpmndi:BPMNEdge>
    </bpmndi:BPMNPlane>
  </bpmndi:BPMNDiagram>
</bpmn:definitions>
